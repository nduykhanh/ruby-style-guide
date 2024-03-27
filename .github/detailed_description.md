### DETAILED DESCRIPTION
#### Meaningful and clean naming for methods, and classes

- Every name you give to a class, function, or variable as well as you are looking for keywords for your article, that is the meaning of that article, so it needs to reflect the content.
- You don't need to describe the content in too detail, just present the basic purpose, and does not make sense if the function of the method or the class has been updated. Recommended that should not be over 50 characters.
- Make sure other people can't mistake the meaning of a method or variable’s name

*Before choosing a name for a variable or method, you should :*
- *Make a list of 3-5 names*
- *Analyze which name will be the most appropriate*
- *The best name when there is only 1 meaning*

#Methods
- Bad: run, process, execute
- Good: calculate_total_price, fetch_data, update_status

#Classes
- Bad: Thing, Data, Manager
- Good: UserProfile, PaymentGateway, CustomerService

#Variables
- Bad: x, temp, var1
- Good: user_name, order_total, product_list

#### Keep the number of instance variables shared between each controller and view as low as possible

- When a lot of instance variables are shared between a controller and a view makes the code difficult to manage. It’s also potentially dangerous to performance because you can end up making duplicate calls on associations unknowingly.
- Instead, your controller should only manage <= 5 instance variables (use decorator, hash, OpenStruct,...). That way, all calls to associations are loaded “on-demand”, and can be instance-variable-cached in a single place.

#bad
```ruby
class ProductsController < ApplicationController
  def show
    @product = Product.find(params[:id])  # Model object (good)

    # Many instance variables for individual data points (bad)
    @product_name = @product.name
    @product_price = @product.price
    @product_description = @product.description
    @image_url = @product.image_url  # Assuming product has an image
    @average_rating = ProductReview.average_rating(@product.id) 
  end
end
```

#good
```ruby
class ProductsController < ApplicationController
  def show
    @product = Product.find(params[:id])
    @average_rating = ProductReview.average_rating(@product.id) 
  end
end
```

####  Don't use acronyms for the naming of variables, methods, and classes, magic number
Explanation:

- Acronyms like HTTPReq are not easily understandable and may lead to confusion, especially for developers not familiar with the acronym.
- Magic numbers like 0.2 are used directly in the code without explanation, making it unclear what the number represents.
#bad
```ruby
class HTTPReq
  def send_req(url, method)
    # Some code here
  end
end

class Calculator
  def calculate_total(price, quantity)
    tax_rate = 0.2
    total = (price * quantity) * (1 + tax_rate)
  end
end
```

#good
```ruby
class HttpRequest
  def send_request(url, method)
    # Some code here
  end
end

class Calculator
  TAX_RATE = 0.2

  def calculate_total(price, quantity)
    total = (price * quantity) * (1 + TAX_RATE)
  end
end
```




#### One method should only perform one task

1. Try to list all the tasks the code is doing
2. Some of these tasks could easily become separate functions (or classes)
3. The rest should be separated from each other by functional groups by blank lines (validate, variable assignment, query)

#bad
```ruby
def process_order(order)
  update_inventory_and_calculate_total(order)
end
```

#good
```ruby
def process_order(order)
  update_inventory(order)
  calculate_total(order)
  send_confirmation_email(order)
end
```


####  Add prefixes “min”, and “max” for variables with limited meaning

- If the minimum or maximum value is a well-defined constant, you can declare it separately.
```ruby
# Minimum password length
MIN_PASSWORD_LENGTH = 8
# Maximum password length
MAX_PASSWORD_LENGTH = 16
```

#### Define Predicate Methods with a “?” and defective verb (is, has,..) for boolean variables
- In Ruby, we have a convention for the methods which return true or false. These methods are also known as predicate methods and the convention is to end their name with a question mark (?).
- In most programming languages, you’ll see methods or variable names defined like is_valid or is_paid, etc. Ruby discourages this style and encourages them in a more human language way like object.valid? or fee.paid? (notice no is_ prefix, as well) keeping in line with the idiomaticness and readability of Ruby.


#bad
```ruby
def ship_ready
  if status == 'ready'
    true
  else
    false
  end
end
```


#good
```ruby
def ready_to_ship?
  status == 'ready'
end
```

#### Use “explaining variable”, “summary variable” to separate complex code

Explaining variables and summary variables are techniques used to improve the readability and understandability of complex code by breaking it down into smaller, more manageable parts. Here's an example demonstrating their use:

Example:
```ruby
#bad
User.active.where(id: params[:user_ids]).map{ |user| { user.id => user.name }}
⇒ 
#good
users = User.active.where(id: params[:user_ids])
users.map { |user| { user.id => user.name }}
```

####  A variable cannot change its data type or change its value inconsistently

This approach keeps the code clear and avoids potential bugs or confusion caused by inconsistent data type or value changes.
#bad
```ruby
def calculate_total(price, quantity)
  total = price * quantity
  
  if total > 100
    total = "High"  # Changing data type inconsistently
  else
    total = 0       # Changing value inconsistently
  end
  
  total
end
```

#good
```ruby
def calculate_total(price, quantity)
  total = price * quantity
  
  if total > 100
    discount_threshold = true
  else
    discount_threshold = false
  end
  
  total, discount_threshold
end
```



#### Avoid using Nested conditionals (replace by Guard clauses, if/elsif,...)
it's generally preferable to avoid nested conditionals and use alternatives like guard clauses or separate conditionals (if/elsif) to improve code readability and maintainability. By breaking down complex logic into simpler, more manageable parts, you can make your code more understandable and easier to work with.

#bad
```ruby
def validate_user_input(input)
  if input.nil? || input.empty?
    return false
  else
    if input.length > 10
      return false
    else
      return true
    end
  end
end
```

#good
```ruby
def validate_user_input(input)
  return false if !input || input.empty?

  input.length <= 9
end
```

#### No dead code

- Dead code is code that is never exercised during the execution of the application (files, methods, variables,...). Less code relieves the mental burden and reduces time wasted while working in a codebase.
- Only remove dead code when you are confident it is no longer used (deeper investigation and use of analysis tools)

#bad
```ruby
def calculate_total(price, quantity)
  total = price * quantity
  apply_discount(total)
  total
end

def apply_discount(total)
  if total > 100
    total * 0.9
  else
    total
  end
end

# Unused method, dead code
def unused_method
  # Some implementation
end
```

#good
```ruby
def calculate_total(price, quantity)
  total = price * quantity
  apply_discount(total)
  total
end

def apply_discount(total)
  if total > 100
    total * 0.9
  else
    total
  end
end
```

#### Need to categorize annotations
- TODO: Need to be added or modified in the future
- FIXME: Code with a known bug. Needs fix
- HACK: Not very pretty code. Needs refactoring
- REVIEW: It needs to be reviewed to see if it works as intended
- OPTIMIZE: Need refactoring to improve performance
- NOTE: Leave information about why this happened

#bad
```ruby
# Implement error handling
def process_data(data)
  # Some code here
end
```


#good
```ruby
# TODO: Need to implement error handling for edge cases
def process_data(data)
  # Some code here
end

```


#### No business logic code in the controller, view, helper

- The Controller should only contain the following simple logic: simple query, authentication, session - cookie, render - redirect, manage params,...
- Ex:

#good (simple query)
```ruby 
def show
    @user = User.find(params[:id])
end
```
	
#bad
```ruby
def create
    return 422, 'Failed' if params[:company_id].nil?
    
    return 422, 'Failed' if User.find_by(email: params[:email])

    user = User.new(params[:user])
    return 422, 'Failed' unless user.save

    account = Account.new(email: user.email)
    return 422, 'Failed' unless account.save

    [200, 'OK']
end
```

#good (using service object to refactor)
```ruby
def create
     UserService::Create.new(params).call
end

class UserService::Create
     def initialized(params)
     	 @params = params
     end
     
     def call
     	 return reponse_data_failed if validate_params

	 user = User.new(@params[:user])
	 return reponse_data_failed unless user.save

	 account = Account.new(email: user.email)
	 return reponse_data_failed unless account.save

	 [200, 'OK']
     end
     
     private
     def validate_params
     	 @params[:company_id].nil? || User.find_by(email: @params[:email])
     end
     
     def reponse_data_failed
     	 [422, 'Failed']
     end
end
```

- The View should not contain logic queries database, variable assignment,...
- The Helper should only contain the related simple logic between controller and view are globally shared (render partial, calculate value, format value)


#### Don't duplicate logic code
This violates the DRY (Don't Repeat Yourself) principle, as the logic for calculating the area and volume of a cylinder is duplicated.
	
#bad
```ruby
def calculate_area(radius)
  pi = 3.14
  return pi * radius * radius
end

def calculate_volume(radius, height)
  pi = 3.14
  return pi * radius * radius * height
end
```

#good
```ruby 
def calculate_area(radius)
  Math::PI * radius * radius
end

def calculate_volume(radius, height)
  calculate_area(radius) * height
end
```



#### Validate Data

- Data validation is essential in the application. It helps control input and storage, avoid errors, and ensure system safety. There are three types we need to validate data: Input Form, Model, and Database.
- Ex:
```
Validate email, phone number,... in the Form and Model

Validate length, presence, unique,... in the Database
```
 
#### Don’t use default_scope
While this might seem convenient, it can lead to unexpected behavior and make it harder to work with the model.

#bad
```ruby
class Product < ApplicationRecord
  default_scope { where(active: true) }
end
```

In the good example, instead of using default_scope, we define a named scope called active.

#good
```ruby 
class Product < ApplicationRecord
  scope :active, -> { where(active: true) }
end
```


#### Only use active_record callbacks for the simple logic related to the model

- We should only use callbacks for simple logic related to data formatting, setting default values,... and rarely being changed.
- Besides, We should limit their use because when we add a callback, that is code that will execute before we can respond to a request. If a class defines 10 callbacks, that's 10 blocks of code that must execute before we can respond to the user. Generally, this will make requests take longer. Requests that take longer result in a sluggish experience on the front-end.
- In addition, it is also a cause of deadlock if the inner task executes too long
- Ex:

#good:
```
calculated values, set default values,...
```
#bad:
```
send mail, the processing logic of another model
```

#### Should only use maximum 2 object attribute sublevels (Demeter)
 
- If you have too many sublevels in one model call, it leads to your code being quite cumbersome and difficult to maintain (if one of the chains is changed). 
- Ex:
```ruby
User.company.address (used in > 5 places) → User.company_address

Company.address.city.name → Company.address.city_name
```

#### Using Strong parameters when there is a Mass Assignment
In the good example, we use the user_params private method to whitelist the parameters for mass assignment using strong parameters. This ensures that only the specified parameters are allowed to be assigned, improving security and reducing the risk of mass assignment vulnerabilities.
#bad
```ruby
class UsersController < ApplicationController
  def create
    @user = User.new(params[:user])
    if @user.save
      redirect_to @user
    else
      render 'new'
    end
  end
end
```

#good
```ruby
class UsersController < ApplicationController
  def create
    @user = User.new(user_params)
    if @user.save
      redirect_to @user
    else
      render 'new'
    end
  end
  
  private
  
  def user_params
    params.require(:user).permit(:name, :email, :password)
  end
end
```



#### Use Indexes for frequently queried columns
With the index in place, fetching a user by ID becomes much faster, even as the size of the users table grows.

#bad
```ruby
class UsersController < ApplicationController
  def show
    # Without an index on the id column
    @user = User.find(params[:id])
  end
end
```

#good
```ruby
# Good
class UsersController < ApplicationController
  def show
    @user = User.find(params[:id])
  end
end

# Assuming migration
class AddIndexToUsersId < ActiveRecord::Migration[6.0]
  def change
    add_index :users, :id
  end
end
```

####  No N+1 queries exist in the code
When rendering associated records in a view, always make sure to use eager loading methods like includes to avoid N+1 query problems. This helps in improving the performance of your application and reduces the load on the database server.

Let's assume we have a User model that has many posts. Here's an example of how to handle N+1 query problem in a view:
#bad
```ruby
# Controller
def show
  @users = User.all
end

# View
<% @users.each do |user| %>
  <div>
    <h2><%= user.name %></h2>
    <% user.posts.each do |post| %> <!-- N+1 query problem -->
      <p><%= post.title %></p>
    <% end %>
  </div>
<% end %>
```

#good
```ruby
# Controller
def show
  @users = User.includes(:posts)
end

# View
<% @users.each do |user| %>
  <div>
    <h2><%= user.name %></h2>
    <% user.posts.each do |post| %> <!-- No N+1 query problem -->
      <p><%= post.title %></p>
    <% end %>
  </div>
<% end %>
```
1
