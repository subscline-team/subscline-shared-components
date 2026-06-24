<style global lang="postcss">
  /**
   * もしプロジェクト側でCSS変数が定義されていなくてもshared-componentsを使えるようにするために、
   * 二重管理を承知の上でここに定義している。
   * whereを使って詳細度を下げることでプロジェクト側に上書きさせる。
   */
  :where(:root) {
    --main_color: hsl(187, 60%, 44%);
    --secondary_color: hsl(89, 43%, 55%);
    --tt_color_black: hsl(187, 11%, 18%);
    --tt_color_gray: hsl(186, 8%, 45%);
    --tt_color_light-gray: hsl(186, 11%, 66%);
    --line_color_black: hsl(183, 5%, 53%);
    --line_color_gray: hsl(180, 11%, 73%);
    --bg_color_main: hsl(0, 0%, 93.3%);
    --bg_color_sub1: hsl(0, 0%, 87%);
    --bg_color_sub2: hsl(0, 0%, 96%);
    --attention_color: hsl(349, 100%, 35%);

    --liff_secondary_color: hsl(36, 100%, 43%);
    --liff_text_black: hsl(0, 0%, 0%);
    --liff_text_gray: hsl(0, 0%, 38%);
    --liff_text_light-gray: hsl(0, 0%, 59%);
    --liff_line_color: hsl(0, 0%, 77%);
    --liff_bg_gray: hsl(0, 0%, 93%);
    --liff_bg_black: hsl(0, 0%, 81%);

    /* === コンポーネントの面色・状態色 ===
     * 以前は各コンポーネントの <style> 内に white / hsl(0 0% 9x%) などを直値で
     * 持っていたため、ダークモード(data-color-mode='dark')でも白い面・薄グレーの
     * まま残りコントラストが破綻していた。共通変数化し、ライト既定値は従来の直値と
     * 一致させている(ライト表示は不変)。ダーク値は下の dark ブロックで上書きする。 */
    --surface_bg: hsl(0, 0%, 100%); /* 入力欄・ドロップダウン・モーダル等の白い面 */
    --surface_bg_hover: hsl(0, 0%, 97%); /* 面上のホバー(薄グレー) */
    --surface_bg_active: hsl(0, 0%, 94%); /* 面上のアクティブ/押下 */
    --surface_bg_disabled: hsl(0, 0%, 96%); /* disabled の面 */
    --neutral_bg_hover: hsl(0, 0%, 82%); /* グレー(achromatic)ボタンのホバー */
    --neutral_bg_active: hsl(0, 0%, 78%); /* グレー(achromatic)ボタンの押下 */
    --text_disabled: hsl(0, 0%, 75%); /* disabled の文字 */
    --selected_bg: hsl(187, 60%, 95%); /* 選択中 option の地(メインカラーの淡色) */
    --selected_bg_hover: hsl(187, 60%, 93%);
    --selected_bg_active: hsl(187, 60%, 91%);

    /* 複数のコンポーネントで共有する変数 */
    --one-line-input-height: 2.5em;
    --modal-backdrop-z-index: 1000;
  }

  /**
   * ダークモード。プロジェクト側(on-Line-Subscribe の app.postcss)は同じ変数を
   * より高い詳細度で上書きするためそちらが優先されるが、submodule 単体・カタログ
   * でもダークが成立するよう :where で低詳細度のフォールバック値を持たせる。
   * 既存パレットの値はプロジェクト側 dark 定義と一致させている。
   */
  :where(:root[data-color-mode='dark']) {
    --main_color: hsl(187, 62%, 54%);
    --secondary_color: hsl(89, 45%, 62%);
    --tt_color_black: hsl(190, 24%, 92%);
    --tt_color_gray: hsl(190, 13%, 76%);
    --tt_color_light-gray: hsl(190, 10%, 62%);
    --line_color_black: hsl(190, 12%, 78%);
    --line_color_gray: hsl(190, 10%, 42%);
    --bg_color_main: hsl(220, 19%, 12%);
    --bg_color_sub1: hsl(220, 17%, 17%);
    --bg_color_sub2: hsl(220, 15%, 21%);
    --attention_color: hsl(358, 84%, 68%);

    --surface_bg: hsl(220, 15%, 21%);
    --surface_bg_hover: hsl(220, 14%, 26%);
    --surface_bg_active: hsl(220, 13%, 31%);
    --surface_bg_disabled: hsl(220, 16%, 16%);
    --neutral_bg_hover: hsl(220, 12%, 30%);
    --neutral_bg_active: hsl(220, 12%, 36%);
    --text_disabled: hsl(220, 8%, 50%);
    --selected_bg: hsl(187, 36%, 24%);
    --selected_bg_hover: hsl(187, 36%, 28%);
    --selected_bg_active: hsl(187, 36%, 32%);
  }
</style>
