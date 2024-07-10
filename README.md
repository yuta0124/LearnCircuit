# Circuit
Circuitを使うことで、Viewと同じライフサイクルでEventを受け取って値を返す仕組みを実現でき、かつ、ViewModelと同じスコープで値を持つこよができる。
![image](https://github.com/yuta0124/LearnCircuit/assets/73418568/6df177fc-ae4e-4471-b352-2a4d9a17495d)

## Presenter
`ComposeRuntime`を使用して状態の計算と発行を担う、プレゼンターロジックはComposeUIを出力しないため、これを強制するため`Presenter.present`には`@ComposableTarget("presenter")`が付与されており、違反するとコンパイラが警告を出力する。<br>
ただし、この警告がIDEには表示されないため、ビルドを失敗させるにはgradleに以下を追記する
```
// In build.gradle.kts
kotlin.compilerOptions.allWarningsAsErrors.set(true)
```
Sampleアプリの実装では、`Presenter`を継承したクラスを使わずに以下の様に定義しているが、`Presenter`を継承して`override fun presenter`に実装する形でも特に変わりはない。(多分)
```
@CircuitInject(SampleScreen::class, AppScope::class)
@Composable
fun samplePresenter(): State 
```
プレゼンター内での値の保持には以下のいずれかを利用する?
- `remember`: 再構成にわたって値を記憶する。
- `rememberRetained`: カスタム、再コンポジション、バックスタック、構成変更にわたって値を記憶する、`Navigator`や`Context`などのリーク可能なインスタンスを保持してはならない **基本こいつが良さそう**
- `rememberSaveable`: `rememberRetained`の内容に加えてプロセスの終了(Acitivityの破棄)にわたって値を記憶する、こちらもルールあり

   
## 参考
公式： https://github.com/slackhq/circuit <br>
公式github: https://slackhq.github.io/circuit/ <br>
公式サンプル実装: https://github.com/slackhq/circuit/tree/main/samples<br>
githubで探したプロジェクトサンプル：　https://github.com/yveskalume/compose-circuit-casestudy , https://github.com/chrisbanes/tivi/tree, @CircuitInjectをhiltで利用しているサンプル：https://github.com/Adventech/sabbath-school-android/blob/e0c80ed31d988049ba762de952d0b28df814b8e9/app-tv/src/main/kotlin/app/ss/tv/MainActivity.kt  <br>
