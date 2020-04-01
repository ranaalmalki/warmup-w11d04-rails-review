# Rails Review

For each of the following topics, please answer each of the questions. You can use links and images to support your answer. Once done please make a pull request.

## Has many Through

- [Rails Active Record Intro](https://github.com/sei-entropy/lesson-w11d02-rails-active-record#active-record-associations)
-[Hospital App](https://github.com/sei-entropy/hw-w11d02-rails-hospital)

### Questions

1. What is the role of a join table in a many-to-many relationship?
Many-to-many relationships
A many-to-many relationship occurs when multiple records in a table are associated with multiple records in another table. For example, a many-to-many relationship exists between customers and products: customers can purchase various products, and products can be purchased by many customers.

Relational database systems usually don't allow you to implement a direct many-to-many relationship between two tables. Consider the example of keeping track of invoices. If there were many invoices with the same invoice number and one of your customers inquired about that invoice number, you wouldn't know which number they were referring to. This is one reason for assigning a unique value to each invoice.

To avoid this problem, you can break the many-to-many relationship into two one-to-many relationships by using a third table, called a join table. Each record in a join table includes a match field that contains the value of the primary keys of the two tables it joins. (In the join table, these match fields are foreign keys.) These foreign key fields are populated with data as records in the join table are created from either table it joins.

A typical example of a many-to many relationship is one between students and classes. A student can register for many classes, and a class can include many students.

The following example includes a Students table, which contains a record for each student, and a Classes table, which contains a record for each class. A join table, Enrollments, creates two one-to-many relationships—one between each of the two tables.
![table](https://fmhelp.filemaker.com/help/18/fmp/en/FMP_Help/images/relational.07.06.1.png)

Students table and classes table each with a relationship line to the enrollments join table

The primary key Student ID uniquely identifies each student in the Students table. The primary key Class ID uniquely identifies each class in the Classes table. The Enrollments table contains the foreign keys Student ID and Class ID.

To set up a join table for a many-to-many relationship:
1.Using the example above, create a table named Enrollments. This will be the join table.

2.In the Enrollments table, create a Student ID field and a Class ID field.

Join tables typically hold fields that might not make sense to have in any other table. You can add fields to the Enrollments table, such as a Date field to keep track of when someone started a class, and a Cost field to track how much a student paid to take a class.

3.Create a relationship between the two Student ID fields in the tables. Then create a relationship between the two Class ID fields in the tables.

Using this design, if a student registers for three classes, that student will have one record in the Students table and three records in the Enrollments table—one record for each class the student enrolled in.

Notes 

2. What two columns must be present in a join table?
 foreign key 

3. Given the example below, edit the code to define a has many :through relationship.

    ```ruby
    class Customer < ActiveRecord::Base
    has_many :Product  
    has_many :Purchase  

    end

    class Product < ActiveRecord::Base
    has_many :Customer  
    has_many :Purchase 

    end

    class Purchase < ActiveRecord::Base
    has_many :Product  
        has_many :Customer  

    end
    ```


4. Based on #3, give an example of associating two instances via the join model.

    rana = Customer.create({username: "Rana", age: 23})
coke = Product.create({title: "Diet Coke", location: "Fridge 12th Floor"})
#variant 1
bob_grabbing_the_coke = Purchase.create(user: bob, product: coke)
#variant 2
bob.purchases.create(product: coke) 
#variant 3
coke.purchases.create(user: bob)
    ```


## Devise

- [Devise Lesson](https://github.com/sei-entropy/lesson-w11d03-rails-devise)

### Questions

1. What does the `current_user` method that the Devise gem provides?
s a helper, which allows us to get all info of user who is currently signed in, also we can access the session for this scope user_session

2. What does the `authenticate_user!` method that the Devise gem provides?

is also a helper provided by Devise, has to be located in Application Controller and routes to the login / signup if not authenticated. Do not use skip_before_action if you want authenticate_user! to work.

3. Write a signout link using the `link_to` rails helper and a devise path.
<%= link_to "Sign Out", destroy_user_session(current_user)%>


4. How do I generate a devise model in the terminal?
rails generate devise user

5. What are the trade offs for using a gem for authentication over a handrolled solution? (no real right answer)

Personally I prefer Devise.



## Validations

- [Validations Lesson](https://github.com/sei-entropy/lesson-w11d03-rails-model-validations)

### Questions

1. Which component, of Rails MVC, is responsible for the business logic?
Model

2. Assume the user's age is an optional field.  Write the validation to verify that a User's age is between 13 and 125 (inclusive).

class User < ActiveRecord::Base
  validates :age, numericality: (greater_than_or_equal_to: 13, less_than_or_equal_to: 125)
end

3. What would `user.errors.messages` return (for the above User), if you assigned `user.age = 12`?

{ :age=>["must be greater than or equal to 13"] }



4. Assume you visit "/customers/new" and enter some invalid information.  Given this controller code, what url would your browser be on after pressing "Create Customer"?
/customers/new

``` ruby
class CustomersController < ApplicationController
  def new
    @customer = Customer.new
  end

  def create
    @customer = Customer.new(customer_params)
    if @customer.save
      redirect_to customer_path(@customer)
    else
      render :new
    end
  end
  ...
  private
  def customer_params
    params.require(:customer).permit(:first_name, :last_name, :age)
  end
end
```



5. Give one reason why we might have the similar validations in the browser, model, and database layer of our application.


We validate the same information, which we input in browser, correspond to our model and will be saved in database.