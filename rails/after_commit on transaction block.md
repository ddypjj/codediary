I found after_commit callback on rails active record model is not firing when it calls on active record transaction block.

I call Payment.create on transaction block something like this below:

```ruby
ActiveRecord::Base.transaction do
  Payment.create!(
    data: data
  )
end
```

This is my Payment model callback:

```ruby
class Payment < ApplicationRecord

  after_commit on: [:create] do
    generate_payment_number
  end

  def generate_payment_number
    update(payment_number: "P#{Time.current.to_i}#{id}")
  end
end
```

I found that **generate_payment_number** is not firing.

Why?

Transaction block is call transaction operation on database (i use MySQL). I have to pay attention to active record transaction in rails.

It said like this

> There are two types of callbacks associated with committing and rolling back transactions: **after_commit** and **after_rollback**.
**after_commit** callbacks are called on every record saved or destroyed within a transaction immediately after the transaction is committed.  **after_rollback** callbacks are called on every record saved or destroyed within a transaction immediately after the transaction or savepoint is rolled back.

Thats is the reason that **generate_payment_number** method is not firing.

Solution

1. Remove the ActiveRecord::Base.transaction block or
2. I change my callback to **after_create**. Because after_create is always fire after object creation even is not yet commited
So my model look like this
```ruby
class Payment < ApplicationRecord

  after_create :generate_payment_number

  def generate_payment_number
    update(payment_number: "P#{Time.current.to_i}#{id}")
  end
end
```