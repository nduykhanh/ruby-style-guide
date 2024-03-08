---
name: Rails quality check list
about: Template for self-check and reviewers
title: "[PROJECT_NAME][ISSUES/Number]  Issue name"
labels: ''
assignees: ''

---

## I. Information:
 - Link issues:
 - Description:

## II. Rule:
 - [ ] I reviewed code fix problem at local before create PR follow check list.
 - [ ] **I SELF-REVIEW CAREFULLY ON MY PR AGAIN BEFORE REQUEST TO REVIEWERS**


## III. Check list:

### SUPPORT GEMS

- Convention: [Rubocop](https://github.com/rubocop/rubocop)
- Code smell: [rails_best_practices](https://github.com/flyerhzm/rails_best_practices), [RubyCritic](https://github.com/whitesmith/rubycritic)
- Unit test: [SimpleCov](https://github.com/simplecov-ruby/simplecov)
- Performance: [Bullet](https://github.com/flyerhzm/bullet)
- Security: [Brakeman](https://github.com/presidentbeef/brakeman), [BundlerAudit](https://github.com/rubysec/bundler-audit)

### CHECKLIST CONTENT
(*) Required

#### 1. Functionally & Reliability

- [ ] Self-test function follows the requirements *
- [ ] Handle special cases, and exceptions in methods *
- [ ] No dead code (unused methods, files), gem support: [rails_best_practices](https://github.com/flyerhzm/rails_best_practices)* [(see_desc...)](#1-no-dead-code)

#### 2. Readability & Simplicity

- [ ] Passed [Rubocop](https://github.com/rubocop/rubocop) (default to [Ruby-Style-Guide](https://github.com/CQBinh/ruby-style-guide/blob/master/README-viVN.md), but there may be differences depending on the project) *
- [ ] Keep the number of instance variables shared between each controller and view as low as possible, recommended <= 3 instance variables * [(see_desc...)](#2-keep-the-number-of-instance-variables-shared-between-each-controller-and-view-as-low-as-possible)
- [ ] Don't use acronyms for the naming of variables, methods, and classes *
- [ ] Define Predicate Methods with a ```?``` and defective verb (```is```, ```has```,..) for boolean variables * [(see desc...)](#9-define-predicate-methods-with-a--and-defective-verb-is-has-for-boolean-variables)
- [ ] Add prefixes “min”, and “max” for variables with limited meaning *
- [ ] In a comparison expression, the expression to be checked (which changes value frequently) must be placed to ```the left``` *
- [ ] Avoid using ```Nested conditionals``` (replace by ```Guard clauses```, if/elsif,...) *
- [ ] Do not comment out **easily detectable** content when reading the code and don’t use words with general meanings in comments (```it```, ```this```,...) *
- [ ] A method should only perform one task * [(see desc...)](#12-a-method-should-only-perform-one-task)
- [ ] Use ```explaining variable```, ```summary variable``` to separate complex code [(see desc)](#11-use-explaining-variable-summary-variable-to-separate-complex-code)
- [ ] A variable cannot change its ```data type``` or change its value ```inconsistently```
- [ ] Need to categorize annotations (TODO, FIXME, OPTIMIZE,...) [(see desc...)](#10-need-to-categorize-annotations)
- [ ] Meaningful and clean naming for methods and classes (recommended should not be over 50 characters) [(see_desc...)](#3-meaningful-and-clean-naming-for-methods-and-classes)
- [ ] Actions inside methods and classes must be completed ([Tell, don’t ask](https://martinfowler.com/bliki/TellDontAsk.html)) [(see_desc...)](#4-actions-inside-methods-and-classes-must-be-completed-tell-dont-ask)

#### 3. Maintainability, Scalability, Reusability

- [ ] No business logic code in the controller, view, helper (service, decorator, ...) * [(see_desc...)](#5-no-business-logic-code-in-the-controller-view-helper)
- [ ] Don't duplicate logic code (find by [RubyCritic](https://github.com/whitesmith/rubycritic)) *
- [ ] Validate Data (type, format, value, …) for input form, model, database * [(see_desc...)](#6-validate-data)
- [ ] Don’t use [default_scope](https://piechowski.io/post/why-is-default-scope-bad-rails/) *
- [ ] Only use active_record callbacks for the simple logic related to the model and avoid conditional execution [(see_desc...)](#7-only-use-active_record-callbacks-for-the-simple-logic-related-to-the-model)
- [ ] Should only use 1 or 2 (for more than 5 use cases) object attribute sublevels ([Law of Demeter](https://medium.com/@gioch/design-patterns-law-of-demeter-with-rails-49a44a9689fe)) [(see_desc...)](#8-should-only-use-maximum-2-object-attribute-sublevels-demeter)

#### 4. Tests coverage

- [ ] All unit tests should be passed
- [ ] All logic code lines changes should be covered (gem support: [SimpleCov](https://github.com/simplecov-ruby/simplecov))
- [ ] Have unit test for exceptions case

#### 5. Performance

- [ ] No N+1 queries exist in the code * (gem support: [Bullet](https://github.com/flyerhzm/bullet), [jit_preloader](https://github.com/clio/jit_preloader), check by server log)
- [ ] Use Indexes for frequently queried columns
- [ ] Put processes that don't need an immediate response into the background processing
- [ ] Using [rack-mini-profiler](https://github.com/MiniProfiler/rack-mini-profiler), [Benchmark](https://ruby-doc.org/stdlib-2.5.3/libdoc/benchmark/rdoc/Benchmark.html) to calculate the execution time of actions

#### 6. Security

- [ ] Passed [Brakeman](https://github.com/presidentbeef/brakeman) *
- [ ] Using [Strong parameters](https://api.rubyonrails.org/classes/ActionController/StrongParameters.html) when there is a [Mass Assignment](https://guides.rubyonrails.org/v3.2.9/security.html#mass-assignment) *
- [ ] Avoid HTML comments in view templates
- [ ] Suggest using [BundlerAudit](https://github.com/rubysec/bundler-audit) to detect security holes of gems

------------------------------------
## IV. Principles for readable code:

#### 1. Although less code, shorter is good, it should ensure minimal time for you to understand the code

>```
>Write code for others to read, not for machines to read.
>Code is not trying to show intelligence, it should be transparent

*For example, you write 1 line code but it takes 1 hour to read and understand it, while you code 2-3 lines but only need 10-20 mins to understand that function, we should choose the 2nd way, or you can add explain (comment code) to make it easier to understand and visualize what you do*

#### 2. Make sure other people can't mistake the meaning of a method or variable’s name

*Before choosing a name for a variable or method, you should :*
- *Make a list of 3-5 names*
- *Analyze which name will be the most appropriate*
- *The best name when there is only 1 meaning*

#### 3. Unify a coding style and use it throughout the project

>```
>Everyone in a team must write the same code,
>So that when looking at the code, it is impossible to identify the owner

#### 4. Order the blocks of the if/else statement so that the recognizable case/ simple solution first

#### 5. Remove variables that are only used once, have no effect on breaking a complex expression, and have no explanatory meaning to the code

#### 6. The less the variable's value is changed, the easier the code is to read

#### 7. Simple process that can help you code more clearly :

> Coders are writers and Code is a story

1. *Describe what code needs to do, in plain English, as you would to a colleague*
2. *Pay attention to the keywords and phrases used in this description*
3. *Write your code to match this description*

#### 8. For unit tests, it is advisable to give a clear test case name, grouping sub-contents such as data initialization in method, there is a variety of input data

#### 9. Should break a big problem into small problems and solve them one by one

#### 10. Concise code is good but should not degrade performance

------------------------------------
### DETAILED DESCRIPTION

#### 1. No dead code

- Dead code is code that is never exercised during the execution of the application (files, methods, variables,...). Less code relieves the mental burden and reduces time wasted while working in a codebase.
- Only remove dead code when you are confident it is no longer used (deeper investigation and use of analysis tools)

#### 2. Keep the number of instance variables shared between each controller and view as low as possible

- When a lot of instance variables are shared between a controller and a view makes the code difficult to manage. It’s also potentially dangerous to performance because you can end up making duplicate calls on associations unknowingly.
- Instead, your controller should only manage <= 3 instance variables (use decorator, hash, OpenStruct,...). That way, all calls to associations are loaded “on-demand”, and can be instance-variable-cached in a single place.

#### 3. Meaningful and clean naming for methods, and classes

- Every name you give to a class, function, or variable as well as you are looking for keywords for your article, that is the meaning of that article, so it needs to reflect the content.
- You don't need to describe the content in too detail, just present the basic purpose, and does not make sense if the function of the method or the class has been updated. Recommended that should not be over 50 characters.

#### 4. Actions inside methods and classes must be completed (Tell, don’t ask)

- Instead of querying an object about its state, then making a decision on behalf of that object, it's better to send that object a message and let it make the decision.
- Ex: 

#so bad
```ruby
def export_bill(table_number, discount)
    total_price = calculate_service_fee(table_number) * discount
    Printer.export_bill(total_price)
end
```

#so good
```ruby
def export_bill(table_number, discount)
    total_price = calculate_service_fee(table_number, discount)
    Printer.export_bill(total_price)
end
```

#### 5. No business logic code in the controller, view, helper

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
    
    return 422, 'Failed' if User.find_by_email(params[:email])

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
     	 @params[:company_id].nil? || User.find_by_email(@params[:email])
     end
     
     def reponse_data_failed
     	 [422, 'Failed']
     end
end
```

- The View should not contain logic queries database, variable assignment,...
- The Helper should only contain the related simple logic between controller and view are globally shared (render partial, calculate value, format value)

#### 6. Validate Data

- Data validation is essential in the application. It helps control input and storage, avoid errors, and ensure system safety. There are three types we need to validate data: Input Form, Model, and Database.
- Ex:
```
Validate email, phone number,... in the Form and Model

Validate length, presence, unique,... in the Database
```
 
#### 7. Only use active_record callbacks for the simple logic related to the model

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

#### 8. Should only use maximum 2 object attribute sublevels (Demeter)
 
- If you have too many sublevels in one model call, it leads to your code being quite cumbersome and difficult to maintain (if one of the chains is changed). 
- Ex:
```ruby
User.company.address (used in > 5 places) → User.company_address

Company.address.city.name → Company.address.city_name
```

#### 9. Define Predicate Methods with a “?” and defective verb (is, has,..) for boolean variables
- In Ruby, we have a convention for the methods which return true or false. These methods are also known as predicate methods and the convention is to end their name with a question mark (?).
- In most programming languages, you’ll see methods or variable names defined like is_valid or is_paid, etc. Ruby discourages this style and encourages them in a more human language way like object.valid? or fee.paid? (notice no is_ prefix, as well) keeping in line with the idiomaticness and readability of Ruby.
  
#### 10. Need to categorize annotations
- TODO: Need to be added or modified in the future
- FIXME: Code with a known bug. Needs fix
- HACK: Not very pretty code. Needs refactoring
- REVIEW: It needs to be reviewed to see if it works as intended
- OPTIMIZE: Need refactoring to improve performance
- NOTE: Leave information about why this happened

#### 11. Use “explaining variable”, “summary variable” to separate complex code
Example:
```ruby
User.active.where(id: params[:user_ids]).map{ |user| { user.id => user.name }}
⇒ 
users = User.active.where(id: params[:user_ids])
	users.map { |user| { user.id => user.name }}
```

#### 12. A method should only perform one task

1. Try to list all the tasks the code is doing
2. Some of these tasks could easily become separate functions (or classes)
3. The rest should be separated from each other by functional groups by blank lines (validate, variable assignment, query)

## References

Ruby Style Guide: https://github.com/CQBinh/ruby-style-guide/blob/master/README-viVN.md

Categorized community-driven collection of awesome Ruby libraries, tools, frameworks, and software: https://github.com/markets/awesome-ruby

Refactor model: https://codeclimate.com/blog/7-ways-to-decompose-fat-activerecord-models

Example tool to optimize code: https://infinum.com/blog/top-8-tools-for-ruby-on-rails-code-optimization-and-cleanup
1
