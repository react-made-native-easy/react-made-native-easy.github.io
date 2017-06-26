## Testing

With the growing complexities of apps, unit testing is a mandate. Since we are writing code in JS, we can utilize most of the testing frameworks/libraries available out there for `react/web` apps without much changes.

> We recommend using Jest Framework plus some additional utilities like enzyme to make the developer's life easier.

### What's the use of testing UI code?

We all have asked/been asked this question at least once. And agree or not, the term unit testing was not very popular amongst FE developers until recent years. Here are some of the reasons why we should write unit tests.

- It lets you capture bugs before the QA team does:

    We all know that a QA and a dev can never be friends. No dev likes it when QA team finds a bug and tells them to change it. If your code has a good test coverage, there are very less chances to find bugs. Win-win for both sides right?

- Helps other devs understand your code better.

    All the test frameworks have describe block where you define what a method does in a language which any human can understand. Need I say more?

- Refactors your code:

    You will start asking these questions to yourself while coding: *How will I test this code?*, *How do I make sure that each method I wrote can be tested?* If you already ask this questions while writing code, then you are one of the few gems.
- Makes your code modular:

    If your code is tested, there are 99% chances that it is modular, which means that you can easily implement changes in future.

- Makes debugging/implementing changes much, *much* easier :

    There will be cases when you will be required to change a functions logic. *eg, a currency formatter function which return a string.* One way of debugging it would be to go through the UI and check if the desired output is there. Another *smart* way would be to fire your test case, change the test results according to your desired output and __let the test fail__. Now change the logic inside your method to make failed the test pass.

- Does __NOT__ increase the dev time:

    Many of us give this excuse that writing test cases will increase the dev time(Even I used to do this). But believe me it doesn't. During the course of development almost half of the time is spent in debugging/bug fixing .Writing unit tests can decrease the this value from 50% to less than 20%.
