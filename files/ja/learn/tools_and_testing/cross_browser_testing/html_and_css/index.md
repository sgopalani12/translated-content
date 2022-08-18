---
title: 一般的な HTML と CSS の問題への対処
slug: Learn/Tools_and_testing/Cross_browser_testing/HTML_and_CSS
tags:
  - CSS
  - CodingScripting
  - HTML
  - linting
  - クロスブラウザ
  - セレクタ
  - テスト
  - 初心者
  - 学習
  - 記事
translation_of: Learn/Tools_and_testing/Cross_browser_testing/HTML_and_CSS
---
<div>{{LearnSidebar}}</div>

<div>{{PreviousMenuNext("Learn/Tools_and_testing/Cross_browser_testing/Testing_strategies","Learn/Tools_and_testing/Cross_browser_testing/JavaScript", "Learn/Tools_and_testing/Cross_browser_testing")}}</div>

<p class="summary">ここでは、HTML と CSS のコードで発生する可能性のある一般的なクロスブラウザの問題、および問題の発生を防ぐため、または発生する問題を修正するために使用できるツールについて具体的に説明します。これには、コードのリンティング、CSS プレフィックスの処理、問題を追跡するためのブラウザの開発者ツールの使用、ブラウザにサポートを追加するための polyfill の使用、レスポンシブデザイン問題への取り組みなどが含まれます。</p>

<table class="learn-box standard-table">
 <tbody>
  <tr>
   <th scope="row">前提条件:</th>
   <td>主要な <a href="/ja/docs/Learn/HTML">HTML</a>、<a href="/ja/docs/Learn/CSS">CSS</a>、および <a href="/ja/docs/Learn/JavaScript">JavaScript</a> 言語に精通していること。<a href="/ja/docs/Learn/Tools_and_testing/Cross_browser_testing/Introduction">クロスブラウザテストの原則</a>の高水準のアイデア。</td>
  </tr>
  <tr>
   <th scope="row">目標:</th>
   <td>一般的な HTML と CSS のクロスブラウザの問題を診断し、それらを修正するための適切なツールとテクニックを使うことができるようにする。</td>
  </tr>
 </tbody>
</table>

<h2 id="HTML_と_CSS_の問題">HTML と CSS の問題</h2>

<p>一部の HTML と CSS の問題<span class="tlid-translation translation" lang="ja"><span title="">は、両方の言語がかなり単純で、コードがうまく作成され、効率的であり、ページ上に「機能の目的」を意味的に記述していることを確認するという意味で開発者がそれらについて真剣に考えていないという事実にあります。</span></span>最悪の場合、JavaScript を使用して Web ページのコンテンツとスタイル全体を生成するため、ページにアクセスできなくなり、パフォーマンスが低下します (DOM 要素の生成にはコストがかかります)。他のケースでは、初期の機能がブラウザ間で一貫してサポートされていないため、一部の機能やスタイルが一部のユーザには機能しないことがあります。<br>
 レスポンシブデザインの問題も一般的です。デスクトップブラウザで見栄えの良いサイトはモバイル端末だとひどい経験を提供するかもしれません、内容が読むには小さすぎるか、高精細なアニメーションのせいで遅いでしょう。</p>

<p>HTML/CSS に起因するクロスブラウザエラーを減らす方法を見てみましょう。</p>

<h2 id="まず最初に：一般的な問題を解決する">まず最初に：一般的な問題を解決する</h2>

<p><a href="/ja/docs/Learn/Tools_and_testing/Cross_browser_testing/Introduction#Testingdiscovery">このシリーズの最初の記事</a>では、まずクロスブラウザの問題に集中する前に、デスクトップ/モバイルの最新ブラウザでいくつかテストしてコードが正常に機能するか確認することをお勧めします。</p>

<p><a href="/ja/docs/Learn/HTML/Introduction_to_HTML/Debugging_HTML">HTML のデバッグ</a>および <a href="/ja/docs/Learn/CSS/Introduction_to_CSS/Debugging_CSS">CSS のデバッグ</a>の記事では、HTML/CSS のデバッグに関する基本的なガイダンスをいくつか提供しました。基本に慣れていない場合は、先に進む前に必ずこれらの記事をよく読んでください。</p>

<p>基本的には、HTML と CSS のコードが整形式で、構文エラーがないかどうかをチェックすることです。</p>

<div class="note">
<p><strong>メモ</strong>: CSS と HTML に関する一般的な問題の1つは、異なる CSS ルールが互いに矛盾が生じるときに発生します。 サードパーティのコードを使用している場合、これは特に問題になる可能性があります。たとえば、CSS フレームワークを使用して、それが使用しているクラス名の1つが別の目的ですでに使用されているものと衝突しているとします。 または、ある種のサードパーティ API (たとえば広告バナーの生成) によって生成された HTML に、すでに別の目的で使用されているクラス名または ID が含まれていることもあります。これが起こらないようにするには、最初に使用しているツールを調べて、それらを中心にコードを設計する必要があります。また、"名前空間"  CSS も価値があります。ウィジェットがある場合は、それが明確なクラスを持っていることを確認してから、このクラスでウィジェット内の要素を選択するセレクタを起動します。そうすれば競合は起こりにくくなります。例えば、 <code>.audio-player ul a</code> です。</p>
</div>

<h3 id="検証">検証</h3>

<p>HTML の検証では、すべてのタグが適切に閉じられてネストされていること、DOCTYPE を使用していること、およびタグを正しい目的で使用していることを確認します。良い戦略はコードを定期的に検証することです。これを可能にするサービスの 1 つに、W3C <a href="https://validator.w3.org/">マークアップ検証サービス</a>があります。これを使用すると、コードを指定してエラーのリストを返すことができます。</p>

<p><img alt="The HTML validator homepage" src="https://mdn.mozillademos.org/files/12441/validator.png" style="display: block; margin: 0px auto;"></p>

<p>CSS にも同様の話があります — プロパティ名が正しくつづられていること、プロパティ値が正しくつづられていて、それらが使われているプロパティに対して有効であること、中括弧を見逃していないということです。この目的のために、W3C には <a href="http://jigsaw.w3.org/css-validator/">CSS Validator</a> も用意されています。</p>

<h3 id="Linters">Linters</h3>

<p>取りうるもう一つの良い選択肢は、エラーを指摘するだけでなく、CSS の悪いプラクティスについての警告、および他の点にもフラグを立てることができる、いわゆる Linter アプリケーションです。Linter は一般的に、エラー/警告の報告においてより厳格またはより緩やかになるようにカスタマイズできます。</p>

<p>オンラインリンターアプリケーションは多数ありますが、そのうち最良のものはおそらく <a href="https://www.dirtymarkup.com/">Dirty Markup</a> (HTML、CSS、JavaScript)、および <a href="http://csslint.net/">CSS Lint</a> (CSS のみ) です。これらはコードをウィンドウに貼り付けることができ、十字でどんなエラーにでもフラグを立てるでしょう、そしてそれは問題が何であるかを知らせるエラーメッセージを得るためにそれから隠されることができます。Dirty Markup では、<em>Clean</em> ボタンを使用してマークアップを修正することもできます。</p>

<p><img alt="" src="https://mdn.mozillademos.org/files/14113/dirty-markup.png" style="border-style: solid; border-width: 1px; display: block; height: 203px; margin: 0px auto; width: 1204px;"></p>

<p>However, it is not very convenient to have to copy and paste your code over to a web page to check its validity several times. What you really want is a linter that will fit into your standard workflow with the minimum of hassle.</p>

<p>Many code editors have linter plugins. Github's <a href="https://atom.io/">Atom</a> code editor for example has a rich plugin ecosystem available, with many linting options. To show you an example of how such plugins generally work:</p>

<ol>
 <li>Install Atom (if you haven't got an up-to-date version already installed) — download it from the Atom page linked above.</li>
 <li>Go to Atom's <em>Preferences...</em> dialog (e.g. by Choosing <em>Atom &gt; Preferences...</em> on Mac, or <em>File &gt; Preferences...</em> on Windows/Linux) and choose the <em>Install</em> option in the left hand menu.</li>
 <li>In the <em>Search packages</em> text field, type "lint" and press Enter/Return to search for linting-related packages.</li>
 <li>You should see a package called <strong>lint</strong> at the top of the list. Install this first (using the <em>Install</em> button), as other linters rely on it to work. After that, install the <strong>linter-csslint</strong> plugin for linting CSS, and the <strong>linter-tidy</strong> plugin for linting HTML.</li>
 <li>After the packages have finished installing, try loading up an HTML file and a CSS file: you'll see any issues highlighted with green (for warnings) and red (for errors) circles next to the line numbers, and a separate panel at the bottom provides line numbers, error messages, and sometimes suggested values or other fixes.</li>
</ol>

<p><img alt="" src="https://mdn.mozillademos.org/files/14109/atom-htmltidy.png" style="display: block; margin: 0 auto;"><img alt="" src="https://mdn.mozillademos.org/files/14107/atom-csslint.png" style="display: block; height: 516px; margin: 0px auto; width: 700px;"></p>

<p>Other popular editors have similar linting packages available. For example, see:</p>

<ul>
 <li><a href="/ja/docs/Learn/Tools_and_testing/Cross_browser_testing/www.sublimelinter.com/">SublimeLinter</a> for Sublime Text</li>
 <li><a href="https://sourceforge.net/projects/notepad-linter/">Notepad++ linter</a></li>
</ul>

<h3 id="Browser_developer_tools">Browser developer tools</h3>

<p>The developer tools built into most browsers also feature useful tools for hunting down errors, mainly for CSS.</p>

<div class="note">
<p><strong>メモ</strong>: ブラウザが不正な形式のマークアップを自動的に修正しようとするため、HTML エラーは開発ツールではそれほど簡単には表示されない傾向があります。W3C バリデータは HTML エラーを取得するための最良の方法です — 上の <a href="#validation">Validation</a> を見てください。</p>
</div>

<p>As an example, in Firefox the CSS inspector will show CSS declarations that aren't applied crossed out, with a warning triangle. Hovering the warning triangle will provide a descriptive error message:</p>

<p><img alt="" src="https://mdn.mozillademos.org/files/14111/css-message-devtools.png" style="display: block; height: 311px; margin: 0px auto; width: 451px;"></p>

<p>Other browser devtools have similar features.</p>

<h2 id="Common_cross_browser_problems">Common cross browser problems</h2>

<p>Now let's move on to look at some of the most common cross browser HTML and CSS problems. The main areas we'll look at are lack of support for modern features, and layout issues.</p>

<h3 id="Older_browsers_not_supporting_modern_features">Older browsers not supporting modern features</h3>

<p>This is a common problem, especially when you need to support old browsers (such as old IE versions) or you are using features that are implemented using CSS prefixes. In general, most core HTML and CSS functionality (such as basic HTML elements, CSS basic colors and text styling) works across most browsers you'll want to support; more problems are uncovered when you start wanting to use newer features such as <a href="/ja/docs/Learn/CSS/CSS_layout/Flexbox">Flexbox</a>, or <a href="/ja/docs/Web/Apps/Fundamentals/Audio_and_video_delivery">HTML5 video/audio</a>, or even more nascent, <a href="/ja/docs/Learn/CSS/CSS_layout/Grids#Native_CSS_Grids_with_Grid_Layout">CSS Grids</a> or <a href="/ja/docs/Learn/CSS/Styling_boxes/Advanced_box_effects#-webkit-background-clip_text">-webkit-background-clip: text</a>.</p>

<p>Once you've identified a list of potential problem technologies you will be using, it is a good idea to research what browsers they are supported in, and what related techniques are useful. See <a href="#finding_help">Finding help</a> below.</p>

<h4 id="HTML_fallback_behaviour">HTML fallback behaviour</h4>

<p>Some problems can be solved by just taking advantage of the natural way in which HTML/CSS work.</p>

<p>Unrecognised HTML elements are treated by the browser as anonymous inline elements (effectively inline elements with no semantic value, similar to {{htmlelement("span")}} elements). You can still refer to them by their names, and style them with CSS, for example — you just need to make sure they are behaving as you want them to, for example setting <code>display: block;</code> on all of the new semantic elements (such as {{htmlelement("article")}}, {{htmlelement("aside")}}, etc.), but only in old versions of IE that don't recognise them (so, IE 8 and lower). This way new browsers can just use the code as normal, but older IE versions will be able to style these elements too.</p>

<div class="note">
<p><strong>Note</strong>: See <a href="#ie_conditional_comments">IE conditional comments</a> for the best way to do this.</p>
</div>

<p>More complex elements like HTML <code><a href="/ja/docs/Web/HTML/Element/video">&lt;video&gt;</a></code>, <code><a href="/ja/docs/Web/HTML/Element/audio">&lt;audio&gt;</a></code>, and <code><a href="/ja/docs/Web/HTML/Element/canvas">&lt;canvas&gt;</a></code> (and other features besides) have natural mechanisms for fallbacks to be added, which work on the same principle as described above. You can add fallback content in between the opening and closing tags, and non-supporting browsers will effectively ignore the outer element and run the nested content.</p>

<p>For example:</p>

<pre class="brush: html language-html"><code class="language-html"><span class="tag token"><span class="tag token"><span class="punctuation token">&lt;</span>video</span> <span class="attr-name token">id</span><span class="attr-value token"><span class="punctuation token">=</span><span class="punctuation token">"</span>video<span class="punctuation token">"</span></span> <span class="attr-name token">controls</span> <span class="attr-name token">preload</span><span class="attr-value token"><span class="punctuation token">=</span><span class="punctuation token">"</span>metadata<span class="punctuation token">"</span></span> <span class="attr-name token">poster</span><span class="attr-value token"><span class="punctuation token">=</span><span class="punctuation token">"</span>img/poster.jpg<span class="punctuation token">"</span></span><span class="punctuation token">&gt;</span></span>
  <span class="tag token"><span class="tag token"><span class="punctuation token">&lt;</span>source</span> <span class="attr-name token">src</span><span class="attr-value token"><span class="punctuation token">=</span><span class="punctuation token">"</span>video/tears-of-steel-battle-clip-medium.mp4<span class="punctuation token">"</span></span> <span class="attr-name token">type</span><span class="attr-value token"><span class="punctuation token">=</span><span class="punctuation token">"</span>video/mp4<span class="punctuation token">"</span></span><span class="punctuation token">&gt;</span></span>
  <span class="tag token"><span class="tag token"><span class="punctuation token">&lt;</span>source</span> <span class="attr-name token">src</span><span class="attr-value token"><span class="punctuation token">=</span><span class="punctuation token">"</span>video/tears-of-steel-battle-clip-medium.webm<span class="punctuation token">"</span></span> <span class="attr-name token">type</span><span class="attr-value token"><span class="punctuation token">=</span><span class="punctuation token">"</span>video/webm<span class="punctuation token">"</span></span><span class="punctuation token">&gt;</span></span>
  <span class="tag token"><span class="tag token"><span class="punctuation token">&lt;</span>source</span> <span class="attr-name token">src</span><span class="attr-value token"><span class="punctuation token">=</span><span class="punctuation token">"</span>video/tears-of-steel-battle-clip-medium.ogg<span class="punctuation token">"</span></span> <span class="attr-name token">type</span><span class="attr-value token"><span class="punctuation token">=</span><span class="punctuation token">"</span>video/ogg<span class="punctuation token">"</span></span><span class="punctuation token">&gt;</span></span>
  <span class="comment token">&lt;!-- Flash fallback --&gt;</span>
  <span class="tag token"><span class="tag token"><span class="punctuation token">&lt;</span>object</span> <span class="attr-name token">type</span><span class="attr-value token"><span class="punctuation token">=</span><span class="punctuation token">"</span>application/x-shockwave-flash<span class="punctuation token">"</span></span> <span class="attr-name token">data</span><span class="attr-value token"><span class="punctuation token">=</span><span class="punctuation token">"</span>flash-player.swf?videoUrl=video/tears-of-steel-battle-clip-medium.mp4<span class="punctuation token">"</span></span> <span class="attr-name token">width</span><span class="attr-value token"><span class="punctuation token">=</span><span class="punctuation token">"</span>1024<span class="punctuation token">"</span></span> <span class="attr-name token">height</span><span class="attr-value token"><span class="punctuation token">=</span><span class="punctuation token">"</span>576<span class="punctuation token">"</span></span><span class="punctuation token">&gt;</span></span>
     <span class="tag token"><span class="tag token"><span class="punctuation token">&lt;</span>param</span> <span class="attr-name token">name</span><span class="attr-value token"><span class="punctuation token">=</span><span class="punctuation token">"</span>movie<span class="punctuation token">"</span></span> <span class="attr-name token">value</span><span class="attr-value token"><span class="punctuation token">=</span><span class="punctuation token">"</span>flash-player.swf?videoUrl=video/tears-of-steel-battle-clip-medium.mp4<span class="punctuation token">"</span></span> <span class="punctuation token">/&gt;</span></span>
     <span class="tag token"><span class="tag token"><span class="punctuation token">&lt;</span>param</span> <span class="attr-name token">name</span><span class="attr-value token"><span class="punctuation token">=</span><span class="punctuation token">"</span>allowfullscreen<span class="punctuation token">"</span></span> <span class="attr-name token">value</span><span class="attr-value token"><span class="punctuation token">=</span><span class="punctuation token">"</span>true<span class="punctuation token">"</span></span> <span class="punctuation token">/&gt;</span></span>
     <span class="tag token"><span class="tag token"><span class="punctuation token">&lt;</span>param</span> <span class="attr-name token">name</span><span class="attr-value token"><span class="punctuation token">=</span><span class="punctuation token">"</span>wmode<span class="punctuation token">"</span></span> <span class="attr-name token">value</span><span class="attr-value token"><span class="punctuation token">=</span><span class="punctuation token">"</span>transparent<span class="punctuation token">"</span></span> <span class="punctuation token">/&gt;</span></span>
     <span class="tag token"><span class="tag token"><span class="punctuation token">&lt;</span>param</span> <span class="attr-name token">name</span><span class="attr-value token"><span class="punctuation token">=</span><span class="punctuation token">"</span>flashvars<span class="punctuation token">"</span></span> <span class="attr-name token">value</span><span class="attr-value token"><span class="punctuation token">=</span><span class="punctuation token">"</span>controlbar=over<span class="entity token" title="&amp;">&amp;amp;</span>image=img/poster.jpg<span class="entity token" title="&amp;">&amp;amp;</span>file=flash-player.swf?videoUrl=video/tears-of-steel-battle-clip-medium.mp4<span class="punctuation token">"</span></span> <span class="punctuation token">/&gt;</span></span>
      <span class="tag token"><span class="tag token"><span class="punctuation token">&lt;</span>img</span> <span class="attr-name token">alt</span><span class="attr-value token"><span class="punctuation token">=</span><span class="punctuation token">"</span>Tears of Steel poster image<span class="punctuation token">"</span></span> <span class="attr-name token">src</span><span class="attr-value token"><span class="punctuation token">=</span><span class="punctuation token">"</span>img/poster.jpg<span class="punctuation token">"</span></span> <span class="attr-name token">width</span><span class="attr-value token"><span class="punctuation token">=</span><span class="punctuation token">"</span>1024<span class="punctuation token">"</span></span> <span class="attr-name token">height</span><span class="attr-value token"><span class="punctuation token">=</span><span class="punctuation token">"</span>428<span class="punctuation token">"</span></span> <span class="attr-name token">title</span><span class="attr-value token"><span class="punctuation token">=</span><span class="punctuation token">"</span>No video playback possible, please download the video from the link below<span class="punctuation token">"</span></span> <span class="punctuation token">/&gt;</span></span>
  <span class="tag token"><span class="tag token"><span class="punctuation token">&lt;/</span>object</span><span class="punctuation token">&gt;</span></span>
  <span class="comment token">&lt;!-- Offer download --&gt;</span>
  <span class="tag token"><span class="tag token"><span class="punctuation token">&lt;</span>a</span> <span class="attr-name token">href</span><span class="attr-value token"><span class="punctuation token">=</span><span class="punctuation token">"</span>video/tears-of-steel-battle-clip-medium.mp4<span class="punctuation token">"</span></span><span class="punctuation token">&gt;</span></span>Download MP4<span class="tag token"><span class="tag token"><span class="punctuation token">&lt;/</span>a</span><span class="punctuation token">&gt;</span></span>
<span class="tag token"><span class="tag token"><span class="punctuation token">&lt;/</span>video</span><span class="punctuation token">&gt;</span></span></code></pre>

<p>This example (taken from <a href="/ja/docs/Web/Apps/Fundamentals/Audio_and_video_delivery/cross_browser_video_player">Creating a cross-browser video player</a>) includes not only a Flash video fallback for older IE versions, but also a simple link allowing you to download the video if even the Flash player doesn't work, so at least the user can still access the video.</p>

<div class="note">
<p><strong>Note</strong>: 3rd party libraries like <a href="http://videojs.com/">Video.js</a> and <a href="https://www.jwplayer.com/">JW Player</a> use such fallback mechanisms to provide cross-browser support.</p>
</div>

<p>HTML5 form elements also exhibit fallback qualities — HTML5 introduced some special <code><a href="/ja/docs/Web/HTML/Element/input">&lt;input&gt;</a></code> types for inputting specific information into forms, such as times, dates, colors, numbers, etc. These are very useful, particularly on mobile platforms, where providing a pain-free way of entering data is very important for the user experience. Supporting platforms provide special UI widgets when these input types are used, such as a calendar widget for entering dates.</p>

<p>The following example shows date and time inputs:</p>

<pre class="brush: html line-numbers language-html"><code class="language-html"><span class="tag token"><span class="tag token"><span class="punctuation token">&lt;</span>form</span><span class="punctuation token">&gt;</span></span>
  <span class="tag token"><span class="tag token"><span class="punctuation token">&lt;</span>div</span><span class="punctuation token">&gt;</span></span>
    <span class="tag token"><span class="tag token"><span class="punctuation token">&lt;</span>label</span> <span class="attr-name token">for</span><span class="attr-value token"><span class="punctuation token">=</span><span class="punctuation token">"</span>date<span class="punctuation token">"</span></span><span class="punctuation token">&gt;</span></span>Enter a date:<span class="tag token"><span class="tag token"><span class="punctuation token">&lt;/</span>label</span><span class="punctuation token">&gt;</span></span>
    <span class="tag token"><span class="tag token"><span class="punctuation token">&lt;</span>input</span> <span class="attr-name token">id</span><span class="attr-value token"><span class="punctuation token">=</span><span class="punctuation token">"</span>date<span class="punctuation token">"</span></span> <span class="attr-name token">type</span><span class="attr-value token"><span class="punctuation token">=</span><span class="punctuation token">"</span>date<span class="punctuation token">"</span></span><span class="punctuation token">&gt;</span></span>
  <span class="tag token"><span class="tag token"><span class="punctuation token">&lt;/</span>div</span><span class="punctuation token">&gt;</span></span>
  <span class="tag token"><span class="tag token"><span class="punctuation token">&lt;</span>div</span><span class="punctuation token">&gt;</span></span>
    <span class="tag token"><span class="tag token"><span class="punctuation token">&lt;</span>label</span> <span class="attr-name token">for</span><span class="attr-value token"><span class="punctuation token">=</span><span class="punctuation token">"</span>time<span class="punctuation token">"</span></span><span class="punctuation token">&gt;</span></span>Enter a time:<span class="tag token"><span class="tag token"><span class="punctuation token">&lt;/</span>label</span><span class="punctuation token">&gt;</span></span>
    <span class="tag token"><span class="tag token"><span class="punctuation token">&lt;</span>input</span> <span class="attr-name token">id</span><span class="attr-value token"><span class="punctuation token">=</span><span class="punctuation token">"</span>time<span class="punctuation token">"</span></span> <span class="attr-name token">type</span><span class="attr-value token"><span class="punctuation token">=</span><span class="punctuation token">"</span>time<span class="punctuation token">"</span></span><span class="punctuation token">&gt;</span></span>
  <span class="tag token"><span class="tag token"><span class="punctuation token">&lt;/</span>div</span><span class="punctuation token">&gt;</span></span>
<span class="tag token"><span class="tag token"><span class="punctuation token">&lt;/</span>form</span><span class="punctuation token">&gt;</span></span></code></pre>

<p>The output of this code is as follows:</p>

<div class="hidden">
<h6 id="Hidden_example">Hidden example</h6>

<pre class="brush: css line-numbers language-css"><code class="language-css"><span class="selector token">label</span> <span class="punctuation token">{</span>
        <span class="property token">float</span><span class="punctuation token">:</span> left<span class="punctuation token">;</span>
        <span class="property token">width</span><span class="punctuation token">:</span> <span class="number token">30</span><span class="token unit">%</span><span class="punctuation token">;</span>
        <span class="property token">text-align</span><span class="punctuation token">:</span> right<span class="punctuation token">;</span>
      <span class="punctuation token">}</span>

      <span class="selector token">input</span> <span class="punctuation token">{</span>
        <span class="property token">float</span><span class="punctuation token">:</span> right<span class="punctuation token">;</span>
        <span class="property token">width</span><span class="punctuation token">:</span> <span class="number token">65</span><span class="token unit">%</span><span class="punctuation token">;</span>
      <span class="punctuation token">}</span>

      <span class="selector token">label, input</span> <span class="punctuation token">{</span>
        <span class="property token">margin-bottom</span><span class="punctuation token">:</span> <span class="number token">20</span><span class="token unit">px</span><span class="punctuation token">;</span>
      <span class="punctuation token">}</span>

      <span class="selector token">div</span> <span class="punctuation token">{</span>
        <span class="property token">clear</span><span class="punctuation token">:</span> both<span class="punctuation token">;</span>
        <span class="property token">margin</span><span class="punctuation token">:</span> <span class="number token">10</span><span class="token unit">px</span><span class="punctuation token">;</span>
      <span class="punctuation token">}</span>

      <span class="selector token">body</span> <span class="punctuation token">{</span>
        <span class="property token">width</span><span class="punctuation token">:</span> <span class="number token">400</span><span class="token unit">px</span><span class="punctuation token">;</span>
        <span class="property token">margin</span><span class="punctuation token">:</span> <span class="number token">0</span> auto<span class="punctuation token">;</span>
      <span class="punctuation token">}</span></code></pre>
</div>

<p> </p>

<pre class="brush: html line-numbers language-html"><code class="language-html"><span class="tag token"><span class="tag token"><span class="punctuation token">&lt;</span>form</span><span class="punctuation token">&gt;</span></span>
      <span class="tag token"><span class="tag token"><span class="punctuation token">&lt;</span>div</span><span class="punctuation token">&gt;</span></span>
        <span class="tag token"><span class="tag token"><span class="punctuation token">&lt;</span>label</span> <span class="attr-name token">for</span><span class="attr-value token"><span class="punctuation token">=</span><span class="punctuation token">"</span>date<span class="punctuation token">"</span></span><span class="punctuation token">&gt;</span></span>Enter a date:<span class="tag token"><span class="tag token"><span class="punctuation token">&lt;/</span>label</span><span class="punctuation token">&gt;</span></span>
        <span class="tag token"><span class="tag token"><span class="punctuation token">&lt;</span>input</span> <span class="attr-name token">id</span><span class="attr-value token"><span class="punctuation token">=</span><span class="punctuation token">"</span>date<span class="punctuation token">"</span></span> <span class="attr-name token">type</span><span class="attr-value token"><span class="punctuation token">=</span><span class="punctuation token">"</span>date<span class="punctuation token">"</span></span><span class="punctuation token">&gt;</span></span>
      <span class="tag token"><span class="tag token"><span class="punctuation token">&lt;/</span>div</span><span class="punctuation token">&gt;</span></span>
      <span class="tag token"><span class="tag token"><span class="punctuation token">&lt;</span>div</span><span class="punctuation token">&gt;</span></span>
        <span class="tag token"><span class="tag token"><span class="punctuation token">&lt;</span>label</span> <span class="attr-name token">for</span><span class="attr-value token"><span class="punctuation token">=</span><span class="punctuation token">"</span>time<span class="punctuation token">"</span></span><span class="punctuation token">&gt;</span></span>Enter a time:<span class="tag token"><span class="tag token"><span class="punctuation token">&lt;/</span>label</span><span class="punctuation token">&gt;</span></span>
        <span class="tag token"><span class="tag token"><span class="punctuation token">&lt;</span>input</span> <span class="attr-name token">id</span><span class="attr-value token"><span class="punctuation token">=</span><span class="punctuation token">"</span>time<span class="punctuation token">"</span></span> <span class="attr-name token">type</span><span class="attr-value token"><span class="punctuation token">=</span><span class="punctuation token">"</span>time<span class="punctuation token">"</span></span><span class="punctuation token">&gt;</span></span>
      <span class="tag token"><span class="tag token"><span class="punctuation token">&lt;/</span>div</span><span class="punctuation token">&gt;</span></span>
    <span class="tag token"><span class="tag token"><span class="punctuation token">&lt;/</span>form</span><span class="punctuation token">&gt;</span></span></code></pre>

<div class="hidden"> </div>

<p>{{ EmbedLiveSample('Hidden_example', '100%', 150) }}</p>

<div class="note">
<p><strong>Note</strong>: You can also see this running live as <a href="http://mdn.github.io/learning-area/tools-testing/cross-browser-testing/html-css/forms-test.html">forms-test.html</a> on GitHub (see the <a href="https://github.com/mdn/learning-area/blob/master/tools-testing/cross-browser-testing/html-css/forms-test.html">source code</a> also).</p>
</div>

<p>If you view the example on a supporting browser like desktop/Android Chrome or iOS Safari, you'll see the special widgets/features in action as you try to input data. On a non-supporting platform such as Firefox or Internet Explorer, the inputs will just fallback to normal text inputs, so at least the user can still enter some information.</p>

<p>Note: Of course, this may not be a great solution for your project's needs — the difference in visual presentation is not great, plus it is harder to guarantee the data will be entered in the format you want it in. For cross browser forms, It is probably better to rely on simple form elements, or selectively using advanced form elements only in supporting browsers, or using a library that provides decent cross browser form widgets, such as <a href="http://jqueryui.com/">jQuery UI</a> or <a href="https://bootstrap-datepicker.readthedocs.io/en/latest/">Bootstrap datepicker</a>.</p>

<h4 id="CSS_fallback_behaviour">CSS fallback behaviour</h4>

<p>CSS is arguably better at fallbacks than HTML. If a browser encounters a declaration or rule it doesn't understand, it just skips it completely without applying it or throwing an error. This might be frustrating for you and your users if such a mistake slips through to production code, but at least it means the whole site doesn't come crashing down because of one error, and if used cleverly you can use it to your advantage.</p>

<p>Let's look at an example — a simple box styled with CSS, which has some styling provided by various CSS3 features:</p>

<p><img alt="" src="https://mdn.mozillademos.org/files/14141/blingy-button.png" style="display: block; margin: 0 auto;"></p>

<div class="note">
<p><strong>Note</strong>: You can also see this example running live on GitHub as <a href="https://github.com/mdn/learning-area/blob/master/tools-testing/cross-browser-testing/html-css/button-with-fallback.html">button-with-fallback.html</a> (also see the <a href="http://mdn.github.io/learning-area/tools-testing/cross-browser-testing/html-css/button-with-fallback.html">source code</a>).</p>
</div>

<p>The button has a number of declarations that style, but the two we are most interested in are as follows:</p>

<pre class="brush: css line-numbers language-css"><code class="language-css"><span class="selector token">button</span> <span class="punctuation token">{</span>
  <span class="number token">...</span>

  <span class="property token">background-color</span><span class="punctuation token">:</span> <span class="hexcode token">#ff0000</span><span class="punctuation token">;</span>
  <span class="property token">background-color</span><span class="punctuation token">:</span> <span class="function token">rgba</span><span class="punctuation token">(</span><span class="number token">255</span><span class="punctuation token">,</span><span class="number token">0</span><span class="punctuation token">,</span><span class="number token">0</span><span class="punctuation token">,</span><span class="number token">1</span><span class="punctuation token">)</span><span class="punctuation token">;</span>
  <span class="property token">box-shadow</span><span class="punctuation token">:</span> inset <span class="number token">1</span><span class="token unit">px</span> <span class="number token">1</span><span class="token unit">px</span> <span class="number token">3</span><span class="token unit">px</span> <span class="function token">rgba</span><span class="punctuation token">(</span><span class="number token">255</span><span class="punctuation token">,</span><span class="number token">255</span><span class="punctuation token">,</span><span class="number token">255</span><span class="punctuation token">,</span><span class="number token">0.4</span><span class="punctuation token">)</span><span class="punctuation token">,</span>
              inset <span class="number token">-1</span><span class="token unit">px</span> <span class="number token">-1</span><span class="token unit">px</span> <span class="number token">3</span><span class="token unit">px</span> <span class="function token">rgba</span><span class="punctuation token">(</span><span class="number token">0</span><span class="punctuation token">,</span><span class="number token">0</span><span class="punctuation token">,</span><span class="number token">0</span><span class="punctuation token">,</span><span class="number token">0.4</span><span class="punctuation token">)</span><span class="punctuation token">;</span>
<span class="punctuation token">}</span>

<span class="selector token">button<span class="pseudo-class token">:hover</span></span> <span class="punctuation token">{</span>
  <span class="property token">background-color</span><span class="punctuation token">:</span> <span class="function token">rgba</span><span class="punctuation token">(</span><span class="number token">255</span><span class="punctuation token">,</span><span class="number token">0</span><span class="punctuation token">,</span><span class="number token">0</span><span class="punctuation token">,</span><span class="number token">0.5</span><span class="punctuation token">)</span><span class="punctuation token">;</span>
<span class="punctuation token">}</span>

<span class="selector token">button<span class="pseudo-class token">:active</span></span> <span class="punctuation token">{</span>
  <span class="property token">box-shadow</span><span class="punctuation token">:</span> inset <span class="number token">1</span><span class="token unit">px</span> <span class="number token">1</span><span class="token unit">px</span> <span class="number token">3</span><span class="token unit">px</span> <span class="function token">rgba</span><span class="punctuation token">(</span><span class="number token">0</span><span class="punctuation token">,</span><span class="number token">0</span><span class="punctuation token">,</span><span class="number token">0</span><span class="punctuation token">,</span><span class="number token">0.4</span><span class="punctuation token">)</span><span class="punctuation token">,</span>
              inset <span class="number token">-1</span><span class="token unit">px</span> <span class="number token">-1</span><span class="token unit">px</span> <span class="number token">3</span><span class="token unit">px</span> <span class="function token">rgba</span><span class="punctuation token">(</span><span class="number token">255</span><span class="punctuation token">,</span><span class="number token">255</span><span class="punctuation token">,</span><span class="number token">255</span><span class="punctuation token">,</span><span class="number token">0.4</span><span class="punctuation token">)</span><span class="punctuation token">;</span>
<span class="punctuation token">}</span></code></pre>

<p>Here we are providing an <a href="/ja/docs/Web/CSS/color_value#rgba()">RGBA</a> {{cssxref("background-color")}} that changes opacity on hover to give the user a hint that the button is interactive, and some semi-transparent inset {{cssxref("box-shadow")}} shades to give the button a bit of texture and depth. The trouble is that RGBA colors and box shadows don't work in IE versions older than 9 — in older versions the background just wouldn't show up at all so the text would be unreadable, no good at all!</p>

<p><img alt="" src="https://mdn.mozillademos.org/files/14135/unreadable-button.png" style="display: block; margin: 0 auto;"></p>

<p>To sort this out, we have added a second <code>background-color</code> declaration, which just specifies a hex color — this is supported way back in really old browsers, and acts as a fallback if the modern shiny features don't work. What happens is a browser visiting this page first applies the first <code>background-color</code> value; when it gets to the second <code>background-color</code> declaration, it will override the initial value with this value if it supports RGBA colors. If not, it will just ignore the entire declaration and move on.</p>

<div class="note">
<p><strong>Note</strong>: The same is true for other CSS features like <a href="/ja/docs/Web/CSS/Media_Queries/Using_media_queries">media queries</a>, <code><a href="/ja/docs/Web/CSS/@font-face">@font-face</a></code> and <code><a href="/ja/docs/Web/CSS/@supports">@supports</a></code> blocks — if they are not supported, the browser just ignores them.</p>
</div>

<h4 id="IE_conditional_comments">IE conditional comments</h4>

<p>IE conditional comments are a modified proprietary HTML comment syntax, which can be used to selectively apply HTML code to different versions of IE. This has proven to be a very effective mechanism for fixing cross browser bugs. The syntax looks like this:</p>

<pre class="brush: html line-numbers language-html"><code class="language-html"><span class="comment token">&lt;!--[if lte IE 8]&gt;
  &lt;script src="ie-fix.js"&gt;&lt;/script&gt;
  &lt;link href="ie-fix.css" rel="stylesheet" type="text/css"&gt;
&lt;![endif]--&gt;</span></code></pre>

<p>This block will apply the IE-specific CSS and JavaScript only if the browser viewing the page is IE 8 or older. <code>lte</code> means "less than or equal to", but you can also use lt, gt, gte, <code>!</code> for NOT, and other logical syntax.</p>

<div class="note">
<p><strong>Note</strong>: Sitepoint's <span class="l-d-i l-pa2 t-bg-white"><a href="https://www.sitepoint.com/web-foundations/internet-explorer-conditional-comments/">Internet Explorer Conditional Comments</a> provides a useful beginner's tutorial/reference that explains the conditional comment syntax in detail.</span></p>
</div>

<p>As you can see, this is especially useful for applying code fixes to old versions of IE. The use case we mentioned earlier (making modern semantic elements styleable in old versions of IE) can be achieved easily using conditional comments, for example you could put something like this in your IE stylesheet:</p>

<pre class="brush: css line-numbers language-css"><code class="language-css"><span class="selector token">aside, main, article, section, nav, figure, figcaption</span> <span class="punctuation token">{</span>
  <span class="property token">display</span><span class="punctuation token">:</span> block<span class="punctuation token">;</span>
<span class="punctuation token">}</span></code></pre>

<p>It isn't that simple, however — you also need to create a copy of each element you want to style in the DOM via JavaScript, for them to be styleable; this is a strange quirk, and we won't bore you with the details here. For example:</p>

<pre class="brush: js line-numbers language-js"><code class="language-js"><span class="keyword token">var</span> asideElem <span class="operator token">=</span> document<span class="punctuation token">.</span><span class="function token">createElement</span><span class="punctuation token">(</span><span class="string token">'aside'</span><span class="punctuation token">)</span><span class="punctuation token">;</span>
 <span class="operator token">...</span></code></pre>

<p>This sounds like a pain to deal with, but fortunately there is a {{glossary("polyfill")}} available that does the necessary fixes for you, and more besides — see <a href="https://github.com/aFarkas/html5shiv">HTML5Shiv</a> for all the details (see <a href="https://github.com/aFarkas/html5shiv#installation">manual installation</a> for the simplest usage).</p>

<h4 id="Selector_support">Selector support</h4>

<p>Of course, no CSS features will apply at all if you don't use the right <a href="/ja/docs/Learn/CSS/Introduction_to_CSS/Selectors">selectors</a> to select the element you want to style! If you just write a selector incorrectly so the styling isn't as expected in any browser, you'll just need to troubleshoot and work out what is wrong with your selector. We find that it is helpful to inspect the element you are trying to style using your browser's dev tools, then look at the DOM tree breadcrumb trail that DOM inspectors tend to provide to see if your selector makes sense compared to it.</p>

<p>For example, in the Firefox dev tools, you get this kind of output at the bottom of the DOM inspector:</p>

<p><img alt="" src="https://mdn.mozillademos.org/files/14139/dom-breadcrumb-trail.png" style="display: block; height: 24px; margin: 0px auto; width: 448px;"></p>

<p>If for example you were trying to use this selector, you'd be able to see that it wouldn't select the input element as desired:</p>

<pre class="brush: css line-numbers language-css"><code class="language-css">form &gt; #date</code></pre>

<p>(The <code>date</code> form input isn't directly inside the <code>&lt;form&gt;</code>; you'd be better off using a general descendant selector instead of a child selector).</p>

<p>However, another issue that appears in versions of IE older than 9 is that none of the newer selectors (mainly pseudo-classes and pseudo-elements like <code><a href="/ja/docs/Web/CSS/:nth-of-type">:nth-of-type</a></code>, <code><a href="/ja/docs/Web/CSS/:not">:not</a></code>, <code><a href="/ja/docs/Web/CSS/::selection">::selection</a></code>, etc.) work. If you want to use these in your CSS and you need to support older IE versions, a good move is to use Keith Clark's <a href="http://selectivizr.com/">Selectivizr</a> library — this is a small JavaScript library that works on top of an existing JavaScript library like <a href="http://jquery.com/">jQuery</a> or <a href="http://mootools.net/">MooTools</a>.</p>

<ol>
 <li>To try this example, make a local copy of <a href="https://github.com/mdn/learning-area/blob/master/tools-testing/cross-browser-testing/html-css/selectivizr-example-start.html">selectivizr-example-start.html</a>. If you look at this running live, you'll see that it contains two paragraphs, one of which is styled. We've selected the paragraph with <code>p:first-child</code>, which won't work in old versions of IE.</li>
 <li>Now download <a href="http://mootools.net/">MooTools</a> and <a href="http://selectivizr.com/">Selectivizr</a>, and save them in the same directory as your sample HTML.</li>
 <li>Put the following code into the head of your HTML document, just before the opening <code>&lt;style&gt;</code> tag:
  <pre class="brush: html line-numbers language-html"><code class="language-html"><span class="tag token"><span class="tag token"><span class="punctuation token">&lt;</span>script</span> <span class="attr-name token">type</span><span class="attr-value token"><span class="punctuation token">=</span><span class="punctuation token">"</span>text/javascript<span class="punctuation token">"</span></span> <span class="attr-name token">src</span><span class="attr-value token"><span class="punctuation token">=</span><span class="punctuation token">"</span>MooTools-Core-1.6.0.js<span class="punctuation token">"</span></span><span class="punctuation token">&gt;</span></span><span class="tag token"><span class="tag token"><span class="punctuation token">&lt;/</span>script</span><span class="punctuation token">&gt;</span></span>
    <span class="comment token">&lt;!--[if (gte IE 6)&amp;(lte IE 8)]&gt;
      &lt;script type="text/javascript" src="selectivizr-min.js"&gt;&lt;/script&gt;
    &lt;![endif]--&gt;</span></code></pre>
 </li>
</ol>

<ol>
 <li> </li>
</ol>

<p>If you try running this in an old version of IE, it should work fine.</p>

<p><img alt="" src="https://mdn.mozillademos.org/files/14137/new-selector-ie7.png" style="border-style: solid; border-width: 1px; display: block; height: 516px; margin: 0px auto; width: 771px;"></p>

<h4 id="Handling_CSS_prefixes">Handling CSS prefixes</h4>

<p>Another set of problems comes with CSS prefixes — these are a mechanism orignally used to allow browser vendors to implement their own version of a CSS (or JavaScript) feature while the technology is in an experimental state, so they can play with it and get it right without conflicting with other browser's implementations, or the final unprefixed implementations. So for example:</p>

<ul>
 <li>Mozilla uses <code>-moz-</code></li>
 <li>Chrome/Opera/Safari use <code>-webkit-</code></li>
 <li>Microsoft uses <code>-ms-</code></li>
</ul>

<p>Here's some examples:</p>

<pre class="brush: css line-numbers language-css"><code class="language-css"><span class="property token">-webkit-transform</span><span class="punctuation token">:</span> <span class="function token">rotate</span><span class="punctuation token">(</span><span class="number token">90</span><span class="token unit">deg</span><span class="punctuation token">)</span><span class="punctuation token">;</span>

<span class="property token">background-image</span><span class="punctuation token">:</span> <span class="function token">-moz-linear-gradient</span><span class="punctuation token">(</span>left<span class="punctuation token">,</span>green<span class="punctuation token">,</span>yellow<span class="punctuation token">)</span><span class="punctuation token">;</span>
<span class="property token">background-image</span><span class="punctuation token">:</span> <span class="function token">-webkit-gradient</span><span class="punctuation token">(</span>linear<span class="punctuation token">,</span>left center<span class="punctuation token">,</span>right center<span class="punctuation token">,</span><span class="function token">from</span><span class="punctuation token">(</span>green<span class="punctuation token">)</span><span class="punctuation token">,</span><span class="function token">to</span><span class="punctuation token">(</span>yellow<span class="punctuation token">)</span><span class="punctuation token">)</span><span class="punctuation token">;</span>
<span class="property token">background-image</span><span class="punctuation token">:</span> <span class="function token">linear-gradient</span><span class="punctuation token">(</span>to right<span class="punctuation token">,</span>green<span class="punctuation token">,</span>yellow<span class="punctuation token">)</span><span class="punctuation token">;</span></code></pre>

<p>The first line shows a {{cssxref("transform")}} property with a <code>-webkit-</code> prefix — this was needed to make transforms work in Chrome, etc. until the feature was finalized and such browsers added a prefix-free version of the property (at the time of writing, Chrome supported both versions).</p>

<p>The last three lines show three different versions of the <code><a href="/ja/docs/Web/CSS/linear-gradient">linear-gradient()</a></code> function, which is used to generate a linear gradient in the background of an element:</p>

<ol>
 <li>The first one has a <code>-moz-</code> prefix, and shows a slightly older version of the syntax (Firefox)</li>
 <li>The second one has a <code>-webkit-</code> prefix, and shows an even older, proprietary version of the syntax (this is actually from a really old version of the WebKit engine).</li>
 <li>The third one has no prefix, and shows the final version of the syntax (included in the <a href="https://drafts.csswg.org/css-images-3/#linear-gradients">CSS Image Values and Replaced Content Module Level 3 spec</a>, which defines this feature).</li>
</ol>

<p>Prefixed features were never supposed to be used in production websites — they are subject to change or removal without warning, and cause cross browser issues. This is particularly a problem when developers decide to only use say, the <code>-webkit- </code>version of a property — meaning that the site won't work in other browsers. This actually happens so much that other browsers have started to implement <code>-webkit-</code> prefixed versions of various CSS properties, so they will work with such code. Usage of prefixes by browser vendors has declined recently precisely because of these types of problems, but there are still some that need attention.</p>

<p>If you insist on using prefixed features, make sure you use the right ones. You can look up what browsers require prefixes on MDN reference pages, and sites like <a href="http://caniuse.com/">caniuse.com</a>. If you are unsure, you can also find out by doing some testing directly in browsers.</p>

<p>Try this simple example:</p>

<ol>
 <li>Open up google.com, or another site that has a prominent heading or other block level element.</li>
 <li>Right/Cmd + click on the element in question and choose Inspect/Inspect element (or whatever the option is in your browser) — this should open up the dev tools in your browser, with the element highlighted in the DOM inspector.</li>
 <li>Look for a feature you can use to select that element. For example, at the time of writing, the main Google logo had an ID of <code>hplogo</code>.</li>
 <li>Store a reference to this element in a variable, for example:
  <pre class="brush: js line-numbers language-js"><code class="language-js"><span class="keyword token">var</span> test <span class="operator token">=</span> document<span class="punctuation token">.</span><span class="function token">getElementById</span><span class="punctuation token">(</span><span class="string token">'hplogo'</span><span class="punctuation token">)</span><span class="punctuation token">;</span></code></pre>
 </li>
</ol>

<ul>
 <li> </li>
 <li>Now try to set a new value for the CSS property you are interested in on that element; you can do this using the <a href="/ja/docs/Web/API/HTMLElement/style">style</a> property of the element, for example try typing these into the JavaScript console:
  <pre class="brush: js line-numbers language-js"><code class="language-js">test<span class="punctuation token">.</span>style<span class="punctuation token">.</span>transform <span class="operator token">=</span> <span class="string token">'rotate(90deg)'</span>
test<span class="punctuation token">.</span>style<span class="punctuation token">.</span>webkitTransform <span class="operator token">=</span> <span class="string token">'rotate(90deg)'</span></code></pre>
 </li>
</ul>

<ol>
 <li> </li>
</ol>

<p>As you start to type the property name representation after the second dot (note that in JavaScript, CSS property names are written in lower camel case, not hyphenated), the JavaScript console should begin to autocomplete the names of the properties that exist in the browser and match what you've written so far. This is useful for finding out what versions of the property are implemented in that browser.</p>

<p>At the time of writing, both Firefox and Chrome implemented <code>-webkit-</code> prefixed and non-prefixed versions of {{cssxref("transform")}}!</p>

<p>Once you've found out which prefixes you need to support, you should write them all out in your CSS, for example:</p>

<pre class="brush: css line-numbers language-css"><code class="language-css"><span class="property token">-ms-transform</span><span class="punctuation token">:</span> <span class="function token">rotate</span><span class="punctuation token">(</span><span class="number token">90</span><span class="token unit">deg</span><span class="punctuation token">)</span><span class="punctuation token">;</span>
<span class="property token">-webkit-transform</span><span class="punctuation token">:</span> <span class="function token">rotate</span><span class="punctuation token">(</span><span class="number token">90</span><span class="token unit">deg</span><span class="punctuation token">)</span><span class="punctuation token">;</span>
<span class="property token">transform</span><span class="punctuation token">:</span> <span class="function token">rotate</span><span class="punctuation token">(</span><span class="number token">90</span><span class="token unit">deg</span><span class="punctuation token">)</span><span class="punctuation token">;</span></code></pre>

<p>This ensures that all browsers that support any of the above forms of the property can make the feature work. It is worth putting the non-prefixed version last, because that will be the most up-to-date version, which you'll want browsers to use if possible. If for example a browser implements both the <code>-webkit-</code> version and the non-prefixed version,  it will first apply the <code>-webkit-</code> version, then override it with the non-prefixed version. You want it to happen this way round, not the other way round.</p>

<p>Of course, doing this for lots of different CSS rules can get really tedious. It is better to use an automation tool to do it for you. And such tools exist:</p>

<p>The <a href="http://leaverou.github.io/prefixfree/">prefix-free JavaScript library</a> can be attached to a page, and will automatically detect what capabilities are possessed by browsers viewing the page and add prefixes as appropriate. It is really easy and convenient to use, although it does have some downsides (see the link above for details), and it is arguable that parsing every stylesheet in your site and add prefixes at run time can be a drain on the computer's processing power for a large site.</p>

<p>Another solution is to add prefixes automatically during development, and this (and other things besides) can be done using tools like <a href="https://github.com/postcss/autoprefixer">Autoprefixer</a> and <a href="http://postcss.org/">PostCSS</a>. These tools can be used in a variety of ways, for example Autoprefixer has an <a href="http://autoprefixer.github.io/">online version</a> that allows you to enter your non-prefixed CSS on the left, and gives you a prefix-added version on the right. You can choose which browsers you want to make sure you support using the notation outlined in <a href="https://github.com/postcss/autoprefixer#options">Autoprefixer options</a>; also see <a href="https://github.com/ai/browserslist#queries">Browserslist queries</a>, which this is based on, for more detail. As an example, the following query will select the last 2 versions of all major browsers and versions of IE above 9.</p>

<pre class="line-numbers language-html"><code class="language-html">last 2 versions, ie &gt; 9</code></pre>

<p>Autoprefixer can also be used in other, more convenient ways — see <a href="https://github.com/postcss/autoprefixer#usage">Autoprefixer usage</a>. For example you can use it with a task runner/build tool such as <a href="http://gulpjs.com/">Gulp</a> or <a href="https://webpack.github.io/">Webpack</a> to automatically add prefixes once development has been done. (Explaining how these work is somewhat beyond the scope of this article.)</p>

<p>You can also use a plugin for a text editor such as Atom or Sublime text. For example, in Atom:</p>

<ol>
 <li>You can install it by going to <em>Preferences &gt; Install</em>, searching for <em>Autoprefixer</em>, then hitting install.</li>
 <li>You can set a browser query by pressing the Autoprefixer <em>Settings</em> button and entering the query in the text field in the <em>Settings</em> section on the page.</li>
 <li>In your code, you can select sections of CSS you want to add prefixes to, open the command pallette (<em>Cmd/Ctrl + Shift + P</em>), then type in Autoprefixer and select the Autoprefixer result that autocompletes.</li>
</ol>

<p>As an example, we entered the following code:</p>

<pre class="brush: css line-numbers language-css"><code class="language-css"><span class="selector token">body</span> <span class="punctuation token">{</span>
  <span class="property token">display</span><span class="punctuation token">:</span> flex<span class="punctuation token">;</span>
<span class="punctuation token">}</span></code></pre>

<p>We highlighted it and ran the Autoprefixer command, and it replaced it with this:</p>

<pre class="brush: css line-numbers language-css"><code class="language-css"><span class="selector token">body</span> <span class="punctuation token">{</span>
  <span class="property token">display</span><span class="punctuation token">:</span> -webkit-box<span class="punctuation token">;</span>
  <span class="property token">display</span><span class="punctuation token">:</span> -ms-flexbox<span class="punctuation token">;</span>
  <span class="property token">display</span><span class="punctuation token">:</span> flex<span class="punctuation token">;</span>
<span class="punctuation token">}</span></code></pre>

<h3 id="Layout_issues">Layout issues</h3>

<p>Another problem that might come up is differences in layouts between browsers. Historically this used to be much more of a problem, but recently, with modern browsers tending to support CSS more consistently, layout issues tend to be more commonly associated with:</p>

<ul>
 <li>Lack of (or differences in) support for modern layout features.</li>
 <li>Layouts not looking good in mobile browsers (i.e. responsive design problems).</li>
</ul>

<div class="note">
<p><strong>Note</strong>: Historically web developers used to use CSS files called resets, which removed all the default browser styling applied to HTML, and then applied their own styles for everything over the top — this was done to make styling on a project more consistent, and reduce possible cross browser issues, especially for things like layout. However, it has more recently been seen as overkill. The best equivalent we have in modern times is <a href="https://necolas.github.io/normalize.css/">normalize.css</a>, a neat bit of CSS that builds slightly on the default browser styling to make things more consistent and fix some layout issues. You are advised to apply normalize.css to all your HTML pages.</p>
</div>

<div class="note">
<p><strong>Note</strong>:  When trying to track down a tricky layout issue, a good technique is to add a brightly colored {{cssxref("outline")}} to the offending element, or all the elements nearby. This makes it a lot easier to see where everything is placed. See <a href="http://www.otsukare.info/2016/10/05/debugging-css" rel="bookmark" title="Permalink to Debug your CSS with outline visualizations.">Debug your CSS with outline visualizations</a> for more details.</p>
</div>

<h4 id="Support_for_new_layout_features">Support for new layout features</h4>

<p>Much of the layout work on the web today is done using <a href="/ja/docs/Learn/CSS/CSS_layout/Floats">floats</a> — this is because floats are well-supported (way back to IE4, albeit with a number of bugs that would also need to be investigated if you were to try to support IE that far back). However, they are not really meant for layout purposes — using floats the way we do is really a hack — and they do have some serious limitations (e.g. see <a href="/ja/docs/Learn/CSS/CSS_layout/Flexbox#Why_Flexbox">Why Flexbox?</a>)</p>

<p>More recently, dedicated layout mechanisms have appeared, like <a href="/ja/docs/Learn/CSS/CSS_layout/Flexbox">Flexbox</a> and <a href="/ja/docs/Learn/CSS/CSS_layout/Grids#Native_CSS_Grids_with_Grid_Layout">CSS Grids</a>, which make common layout tasks far easier and remove such shortcomings. These however are not as well-supported in browsers:</p>

<ul>
 <li>CSS grids are very new; at the time of writing, they were only <a href="http://gridbyexample.com/browsers/">supported</a> in the very newest versions of modern browsers.</li>
 <li>Flexbox is <a href="/ja/docs/Learn/CSS/CSS_layout/Flexbox#Cross_browser_compatibility">well-supported</a> in modern browsers, but provides problems in older browsers. IE 9 doesn't support it at all, and IE 10 and old versions of iOS/desktop Safari respectively support incompatible old versions of the flexbox spec. This results in some interesting browser prefix juggling if you want to try to use flexbox across all these browsers (see <a href="https://dev.opera.com/articles/advanced-cross-browser-flexbox/">Advanced Cross-Browser Flexbox</a> to get an idea).</li>
</ul>

<p>Layout features aren't as easy to provide graceful fallbacks for than simple colors, shadows, or gradients. If layout properties are ignored, your entire design will likely fall to pieces. Because of this, you need to use feature detection to detect whether visiting browsers support those layout features, and selectively apply different layouts depending on the result (we will cover feature detection in detail in a later article).</p>

<p>For example, you could apply a flexbox layout to modern browsers, then instead apply a floated layout to older browsers that don't support flexbox.</p>

<div class="note">
<p><strong>Note</strong>: There is a fairly new feature in CSS called <code><a href="/ja/docs/Web/CSS/@supports">@supports</a></code>, which allows you to implement native feature detection tests.</p>
</div>

<h4 id="Responsive_design_problems">Responsive design problems</h4>

<p>Responsive design is the practice of creating web layouts that change to suit different device form factors — for example different screen widths, orientations (portrait or landscape), or resolutions. A desktop layout for example will look terrible when viewed on a mobile device, so you need to provide a suitable mobile layout using <a href="/ja/docs/Web/CSS/Media_Queries">media queries</a>, and make sure it is applied correctly using <a href="/ja/docs/Mozilla/Mobile/Viewport_meta_tag">viewport</a>. You can find a detailed account of such practices in <a href="/ja/docs/Web/Apps/Progressive/Responsive/responsive_design_building_blocks">The building blocks of responsive design</a>.</p>

<p>Resolution is a big issue too — for example, mobile devices are less likely to need big heavy images than desktop computers, and are more likely to have slower internet connections and possibly even expensive data plans that make wasted bandwidth more of a problem. In addition, different devices can have a range of different resolutions, meaning that smaller images could appear pixellated. There are a number of techniques that allow you to work around such problems, from simple <a href="/ja/Apps/Progressive/Responsive/Mobile_first">mobile first media queries</a>, to more complex <a href="/ja/docs/Learn/HTML/Multimedia_and_embedding/Responsive_images#Resolution_switching_Different_sizes">responsive image techniques</a>.</p>

<p>Another difficulty that can present problems is browser support for the features that make the above techniques possible. media queries are not supported in IE 8 or less, so if you want to use a mobile first layout and have the desktop layout then apply to old IE versions, you'll have to apply a media query {{glossary("polyfill")}} to your page, like <a href="https://code.google.com/archive/p/css3-mediaqueries-js/">css3-mediaqueries-js</a>, or <a href="https://github.com/scottjehl/Respond">Respond.js</a>.</p>

<h2 id="Finding_help">Finding help</h2>

<p>There are many other issues you'll encounter with HTML and CSS; the most important thing to know really is how to find answers online.</p>

<p>Among the best sources of support information are the Mozilla Developer Network (that's where you are now!), <a href="http://stackoverflow.com/">stackoverflow.com</a>, and <a href="http://caniuse.com/">caniuse.com</a>.</p>

<p>To use the Mozilla Developer Network (MDN), most people do a search engine search of the technology they are trying to find information on, plus the term "mdn", for example "mdn HTML5 video". MDN contains several useful types of content:</p>

<ul>
 <li>Reference material with browser support information for client-side web technologies, e.g. the <a href="/ja/docs/Web/HTML/Element/video">&lt;video&gt; reference page</a>.</li>
 <li>Other supporting reference material, e.g. <a href="/ja/docs/Web/HTML/Supported_media_formats">Media formats supported by the HTML audio and video elements</a>.</li>
 <li>Useful tutorials that solve specific problems, for example <a href="/ja/docs/Web/Apps/Fundamentals/Audio_and_video_delivery/cross_browser_video_player">Creating a cross-browser video player</a>.</li>
</ul>

<p><a href="http://caniuse.com/">caniuse.com</a> provides support information, along with a few useful external resource links. For example, see <a href="http://caniuse.com/#search=video">http://caniuse.com/#search=video</a> (you just have to enter the feature you are searching for into the text box).</p>

<p><a href="http://stackoverflow.com/">stackoverflow.com</a> (SO) is a forum site where you can ask questions and have fellow developers share their solutions, look up previous posts, and help other developers. You are advised to look and see if there is an answer to your question already, before posting a new question. For example, we searched for "cross browser html5 video" on SO, and very quickly came up with <a class="question-hyperlink" href="http://stackoverflow.com/questions/16212510/html5-video-with-full-cross-browser-compatibility">HTML5 Video with full cross browser compatibility</a>.</p>

<p>Aside from that, try searching your favourite search engine for an answer to your problem. It is often useful to search for specific error messages if you have them — other developers will be likely to have had the same problems as you.</p>

<h2 id="まとめ">まとめ</h2>

<p>Now you should be familiar with the main types of cross browser HTML and CSS problems that you'll meet in web development, and how to go about fixing them.</p>

<p>{{PreviousMenuNext("Learn/Tools_and_testing/Cross_browser_testing/Testing_strategies","Learn/Tools_and_testing/Cross_browser_testing/JavaScript", "Learn/Tools_and_testing/Cross_browser_testing")}}</p>

<h2 id="このモジュール">このモジュール</h2>

<ul>
 <li><a href="/ja/docs/Learn/Tools_and_testing/Cross_browser_testing/Introduction">Introduction to cross browser testing</a></li>
 <li><a href="/ja/docs/Learn/Tools_and_testing/Cross_browser_testing/Testing_strategies">Strategies for carrying out testing</a></li>
 <li><a href="/ja/docs/Learn/Tools_and_testing/Cross_browser_testing/HTML_and_CSS">Handling common HTML and CSS problems</a></li>
 <li><a href="/ja/docs/Learn/Tools_and_testing/Cross_browser_testing/JavaScript">Handling common JavaScript problems</a></li>
 <li><a href="/ja/docs/Learn/Tools_and_testing/Cross_browser_testing/Accessibility">Handling common accessibility problems</a></li>
 <li><a href="/ja/docs/Learn/Tools_and_testing/Cross_browser_testing/Feature_detection">Implementing feature detection</a></li>
 <li><a href="/ja/docs/Learn/Tools_and_testing/Cross_browser_testing/Automated_testing">Introduction to automated testing</a></li>
 <li><a href="/ja/docs/Learn/Tools_and_testing/Cross_browser_testing/Your_own_automation_environment">Setting up your own test automation environment</a></li>
</ul>