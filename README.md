# taskly-policy

Taskly の規約本文と公開版を GitHub Releases で管理します。表示サイトは [taskly-policy-web](https://github.com/asari194617/taskly-policy-web) です。

## 正本と表示

公開済みの通常 Release と pre-release が版の正本です。各 Release tag が固定するコミットの `manifest.json` に、公開版、施行日、文書の順序と改定情報を記録します。`versions/<文書の版>/<slug>.md` は本文の固定版です。Release の tag と manifest の `version`、本文の SHA-256 を公開処理が照合します。draft Release は表示しません。

通常 Release のうち施行日が過去で最も新しいものが現行版です。pre-release は改正予定版として別に表示します。各文書の保存版 URL は `/slug/<文書の版>/` を維持します。Release tag を使う `/?v=<tag>` と `/slug/?v=<tag>` でも、版の一覧と該当文書を開けます。

`catalog.json` は移行前の管理台帳として残しています。**新しい公開版をこのファイルだけに追加しても公開されません。** 版管理の正本は Release ごとの manifest です。`policies/` は過去からある原文であり、現在の表示本文とは整形が異なります。

## 既存版の移行

最初の2つの `-import` Release は、Taskly の同意記録が参照する `2026-09-22.1` と、2026年9月23日改訂のプライバシーポリシーを、本文を変えずに記録します。旧版と現行版のコミットを別々の tag に固定します。移行版だけは既存本文に front matter がないことを許容します。以降の新しい本文には `title` と `description` の front matter を含め、manifest と一致させます。

継続性声明 `88536b5` には発効日が明記されていないため、文書ごとの `effectiveDate` は `null` のままにします。Release 自体の施行日と混同しません。

## 改訂の手順

1. 本文と提供実態、告知・同意の必要性を確認します。
2. 新しい文書の版を `versions/` に追加します。公開済みファイルは編集しません。`manifest.json` に文書の順序、版、変更有無、SHA-256、Release の施行日を記録します。
3. PR でレビューし、マージしたコミットへ manifest の `version` と同じ tag を付け、GitHub Release を公開します。改正予定版は pre-release にします。draft は公開されません。
4. Release 公開イベントを起点に、表示サイトが GitHub 上の Release 一覧と各 tag のコミットSHAを検証し、R2へ版固定の本文と一覧を公開します。イベントが届かなかった場合に備え、定期的にも照合します。Next.js の再ビルドは不要です。
5. [公開URL](https://policies.taskly.iy0306.com/)で版履歴、本文、Markdown、差分を確認します。

GitHub または公開処理が失敗した場合、R2 の直前の正常データを表示し続けます。保存版の変更・削除は公開処理が拒否します。Taskly の同意を必要とする版の切り替えは、Taskly API の同意版、告知、同意画面と同時にレビューします。
