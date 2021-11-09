# javascript_todo01

todo リストを簡単に作成

## 仕様

PC 用 ToDo リスト

1. Todo をリストに保持する
2. Todo をクリックで、完了となる
3. Todo を右クリックでタスクが消える

## todo のデータ保持

ローカルストレージを使う（ブラウザ上に預ける）
Golang でサーバーサイドをつくったら、Golang でデータ保持を行う

```js
// ローカルストレージからデータを取得
const todos = JSON.parse(localStorage.getItem("todos"));
// ローカルストレージ更新
function updateLS() {
  // li要素を取得
  todosEl = document.querySelectorAll("li");
  // 保存データ
  const todos = [];
  todosEl.forEach((todoEl) => {
    // オブジェクトを配列にpush
    todos.push({
      text: todoEl.innerText,
      completed: todoEl.classList.contains("completed"),
    });
  });
  // ローカルストレージにtodosキーで保存
  localStorage.setItem("todos", JSON.stringify(todos));
}
```

### JSON.parse() メソッド

文字列を JSON として解析し、文字列によって記述されている JavaScript の値やオブジェクトを構築します。

### Storage

Web Storage API の Storage インターフェイスは、特定のドメインのセッションストレージまたはローカルストレージへのアクセス機能を提供して、例えば保存されているデータアイテムを追加、変更、削除することができます。

### Storage.getItem() メソッド

Storage インターフェイスの getItem() メソッドはキーの名称を渡すと、そのキーに対する値を返します。

```js
var aValue = storage.getItem(keyName);
```

### Document.querySelectorAll() メソッド

与えられた CSS セレクターに一致する文書中の要素のリストを示す静的な (ライブではない) NodeList を返します。

```js
elementList = parentNode.querySelectorAll(selectors);
```

### CSSUnparsedValue.forEach() メソッド

指定された関数を、 CSSUnparsedValue オブジェクトのそれぞれの要素に対して実行します。

### HTMLElement.innerText

ノードとその子孫の「レンダリングされた」テキスト内容を示します。ゲッターとして、カーソルで要素の内容を選択しクリップボードにコピーした際のテキストに近いものを取得することができます。

※ innerText は Node.textContent と混同しやすいのですが、両者には重要な違いがあります。基本的に innerText はテキストがレンダリングされる表示を意識しますが、 textContent はそうではありません。

### Element.classList

DOMTokenList で、その要素の class 属性を返します。 class 属性が設定されていない場合や空の場合は、空の DOMTokenList を返します。すなわち、 DOMTokenList の length プロパティが 0 になります。

### DOMTokenList.contains()

Boolean を返します。 true は渡された token がそのリストに含まれていることを表し、そうでなければ false になります。

### Storage.setItem()

キーの名称と値を渡すと、ストレージにキーを追加する、またはキーがすでに存在する場合はキーに対する値を更新します。

### JSON.stringify()

ある JavaScript のオブジェクトや値を JSON 文字列に変換します。置き換え関数を指定して値を置き換えたり、置き換え配列を指定して指定されたプロパティのみを含むようにしたりすることもできます。

## Todo 作成イベントの発火

```js
// Todo入力時に発火する
form.addEventListener('submit', (e) => {
    // デフォルトの動きをキャンセル
    e.preventDefault()

    // Todoを作成
    addTodo()
})

function addTodo(todo) {
    ...
}
```

## Todo を作成

```js
// Todo作成
function addTodo(todo) {
  // 入力文字を取得
  let todoText = input.value;

  if (todo) {
    todoText = todo.text;
  }

  if (todoText) {
    // liリストを作成
    const todoEl = document.createElement("li");
    // タスク完了かチェック
    // todoはオブジェクトを格納でき、completedを持っている
    if (todo && todo.completed) {
      // completedクラスをつける
      todoEl.classList.add("completed");
    }

    todoEl.innerText = todoText;

    // Todoをクリックした時に発火
    todoEl.addEventListener("click", () => {
      // completedクラスがついてたら削除、そうでない場合は付与
      todoEl.classList.toggle("completed");
      // ローカルストレージを更新
      updateLS();
    });

    // 右クリックした時に発火するイベント
    todoEl.addEventListener("contextmenu", (e) => {
      // デフォルトの動きをキャンセル
      e.preventDefault();

      // Todo削除
      todoEl.remove();
      // ローカルストレージを更新
      updateLS();
    });

    // Todoを親要素の子要素として追加
    todosUL.appendChild(todoEl);

    // 入力欄をクリア
    input.value = "";

    // ローカルストレージを更新
    updateLS();
  }
}
```

## Todo データの読み込み

```js
// ローカルストレージからデータを取得
...
// 画面を開いた時にリストを生成する
if(todos) {
    todos.forEach(todo => addTodo(todo))
}
```
