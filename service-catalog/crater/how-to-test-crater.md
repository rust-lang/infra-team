# How to test crater

After you do infra changes to crater, you might want to run a quick e2e test, to
make sure everything continues working as expected.

For an in-depth guide on how to use the crater bot from the `rust-lang/rust`
repository, check the [bot-usage](https://github.com/rust-lang/crater/blob/master/docs/bot-usage.md)
document. However, if you want to run a quick test, here's how to do it:

1. Identify a PR where the bors testing succeeded, like
   [this](https://github.com/rust-lang/rust/pull/131362#issuecomment-2421811741)
   one.
2. Comment on the PR with the following command:

   ```shell
   @craterbot run mode=check-only crates=https://gist.githubusercontent.com/MarcoIeni/3800cdca02ddb30ac98404cafa849c1b/raw/crates start=master#<start_commit> end=try#<end_commit>
   ```

   Example [here](https://github.com/rust-lang/rust/pull/131362#issuecomment-2435412130).

3. You should see the bot reply with a link to the crater [queue](https://crater.rust-lang.org/).
   Example [here](https://github.com/rust-lang/rust/pull/131362#issuecomment-2435412322).
