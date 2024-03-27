# I. Rails Checklist:
(*) When unit tests are required for the project, this is mandatory.
### AUTOMATIC CHECK

- [x] Convention: [Rubocop](https://github.com/rubocop/rubocop)
- [x] Performance: [Bullet](https://github.com/flyerhzm/bullet)
- [ ] Unit test: [SimpleCov](https://github.com/simplecov-ruby/simplecov) *
- [x] Security: [Gitleak](https://github.com/gitleaks/gitleaks), [Brakeman](https://github.com/presidentbeef/brakeman)

### MANUAL CHECK

#### 1. Readability & Simplicity
- [ ] Meaningful and clean naming for methods and classes (recommended should not be over 50 characters) [(see desc...)](https://github.com/voyager-inc/rails-template-project/blob/main/.github/detailed_description.md#meaningful-and-clean-naming-for-methods-and-classes)
- [ ] Keep the number of instance variables shared between each controller and view as low as possible (recommended <= 5 instance variables) [(see desc...)](https://github.com/voyager-inc/rails-template-project/blob/main/.github/detailed_description.md#keep-the-number-of-instance-variables-shared-between-each-controller-and-view-as-low-as-possible)
- [ ] Don't use acronyms for the naming of variables, methods, and classes, magic number [(see desc...)](https://github.com/voyager-inc/rails-template-project/blob/main/.github/detailed_description.md#dont-use-acronyms-for-the-naming-of-variables-methods-and-classes-magic-number)
- [ ] One method should only perform one task [(see desc...)](https://github.com/voyager-inc/rails-template-project/blob/main/.github/detailed_description.md#one-method-should-only-perform-one-task)
- [ ] Add prefixes “min”, and “max” for variables with limited meaning [(see desc...)](https://github.com/voyager-inc/rails-template-project/blob/main/.github/detailed_description.md#add-prefixes-min-and-max-for-variables-with-limited-meaning)
- [ ] Define Predicate Methods with a ? and defective verb (is, has,..) for boolean variables [(see desc...)](https://github.com/voyager-inc/rails-template-project/blob/main/.github/detailed_description.md#define-predicate-methods-with-a--and-defective-verb-is-has-for-boolean-variables)
- [ ] Use explaining variable, summary variable to separate complex code [(see desc...)](https://github.com/voyager-inc/rails-template-project/blob/main/.github/detailed_description.md#use-explaining-variable-summary-variable-to-separate-complex-code)
- [ ] A variable cannot change its data type or change its value inconsistently [(see desc...)](https://github.com/voyager-inc/rails-template-project/blob/main/.github/detailed_description.md#a-variable-cannot-change-its-data-type-or-change-its-value-inconsistently)
- [ ] Avoid using Nested conditionals (replace by Guard clauses, if/elsif,...) [(see desc...)](https://github.com/voyager-inc/rails-template-project/blob/main/.github/detailed_description.md#avoid-using-nested-conditionals-replace-by-guard-clauses-ifelsif)
- [ ] No dead code (unused methods, files), gem support: [rails_best_practices](https://github.com/flyerhzm/rails_best_practices) [(see desc...)](https://github.com/voyager-inc/rails-template-project/blob/main/.github/detailed_description.md#no-dead-code)
- [ ] Do not comment out easily detectable content when reading the code and don’t use words with general meanings in comments (it, this,...), add prefix: TODO, NOTES [(see desc...)](https://github.com/voyager-inc/rails-template-project/blob/main/.github/detailed_description.md#need-to-categorize-annotations)

#### 2. Maintainability, Scalability, Reusability
- [ ] No business logic code in the controller, view, helper (service, decorator, ...) [(see desc...)](https://github.com/voyager-inc/rails-template-project/blob/main/.github/detailed_description.md#no-business-logic-code-in-the-controller-view-helper)
- [ ] Don't duplicate logic code (find by [RubyCritic](https://github.com/whitesmith/rubycritic)) [(see desc...)](https://github.com/voyager-inc/rails-template-project/blob/main/.github/detailed_description.md#dont-duplicate-logic-code)
- [ ] Validate Data (type, format, value, …) for input form, model, database [(see desc...)](https://github.com/voyager-inc/rails-template-project/blob/main/.github/detailed_description.md#validate-data)
- [ ] Don’t use [default_scope](https://piechowski.io/post/why-is-default-scope-bad-rails/) [(see desc...)](https://github.com/voyager-inc/rails-template-project/blob/main/.github/detailed_description.md#dont-use-default_scope)
- [ ] Should only use 1 or 2 object attribute sublevels ([Law of Demeter](https://medium.com/@gioch/design-patterns-law-of-demeter-with-rails-49a44a9689fe)). Ex: User.company.address → User.company_address [(see desc...)](https://github.com/voyager-inc/rails-template-project/blob/main/.github/detailed_description.md#should-only-use-maximum-2-object-attribute-sublevels-demeter)
- [ ] Only use callbacks for the simple, model-related stuff (calculated values, default values, validations) [(see desc...)](https://github.com/voyager-inc/rails-template-project/blob/main/.github/detailed_description.md#only-use-active_record-callbacks-for-the-simple-logic-related-to-the-model)

#### 3. Tests coverage *
- [ ] All unit tests must be passed
- [ ] All code lines changes must be covered (gem support: [SimpleCov](https://github.com/simplecov-ruby/simplecov))
- [ ] The test case shouldn’t duplicate logic (Using before_action, helper, shared_template, ...)
- [ ] Full test cases for happy case, error case, exceptions case

#### 4. Performance
- [ ] No N+1 queries exist in the code (gem support: [Bullet](https://github.com/flyerhzm/bullet), [jit_preloader](https://github.com/clio/jit_preloader)) [(see desc...)](https://github.com/voyager-inc/rails-template-project/blob/main/.github/detailed_description.md#no-n1-queries-exist-in-the-code)
- [ ] Don’t use [default_scope](https://piechowski.io/post/why-is-default-scope-bad-rails/) [(see desc...)](https://github.com/voyager-inc/rails-template-project/blob/main/.github/detailed_description.md#dont-use-default_scope)
- [ ] Use Indexes for frequently queried columns [(see desc...)](https://github.com/voyager-inc/rails-template-project/blob/main/.github/detailed_description.md#use-indexes-for-frequently-queried-columns)
- [ ] Put processes that don't need an immediate response into the background processing (Optional)

#### 5. Security
- [ ] Using [Strong parameters](https://api.rubyonrails.org/classes/ActionController/StrongParameters.html) when there is a [Mass Assignment](https://github.com/voyager-inc/rails-template-project/blob/main/.github/detailed_description.md#using-strong-parameters-when-there-is-a-mass-assignment)
- [ ] Avoid exposing ENV, secret key on source code or logs
- [ ] Avoid public documents.
- [ ] Database password,...: required for strong passwords
1
