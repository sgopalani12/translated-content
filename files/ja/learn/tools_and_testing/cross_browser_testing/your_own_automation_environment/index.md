---
title: テスト自動化環境をセットアップする
slug: Learn/Tools_and_testing/Cross_browser_testing/Your_own_automation_environment
tags:
  - Article
  - Automation
  - Beginner
  - Browser
  - CodingScripting
  - Learn
  - Testing
  - Tools
  - cross browser
  - selenium
translation_of: Learn/Tools_and_testing/Cross_browser_testing/Your_own_automation_environment
---
<div>{{LearnSidebar}}</div>

<div>{{PreviousMenu("Learn/Tools_and_testing/Cross_browser_testing/Automated_testing", "Learn/Tools_and_testing/Cross_browser_testing")}}</div>

<p class="summary">この記事では、Selenium/WebDriver や selenium-webdriver for Node のようなテストライブラリーを使って、自動化環境のインストールとテストを実行する方法を教えます。またあなたのローカルテスト環境と、以前の記事で見てきたような商用アプリとを統合する方法についても見て行きます。</p>

<table class="learn-box standard-table">
 <tbody>
  <tr>
   <th scope="row">前提条件:</th>
   <td><a href="/ja/docs/Learn/HTML">HTML</a>, <a href="/ja/docs/Learn/CSS">CSS</a>,  <a href="/ja/docs/Learn/JavaScript">JavaScript</a> のコア機能、<a href="/ja/docs/Learn/Tools_and_testing/Cross_browser_testing/Introduction">principles of cross browser testing</a>や<a href="/ja/docs/Learn/Tools_and_testing/Cross_browser_testing/Automated_testing">automated testing</a>などの高レベルのアイデアに精通していること。</td>
  </tr>
  <tr>
   <th scope="row">目的:</th>
   <td>Seleniumによるローカルテスト環境のセットアップ方法やSeleniumを使用したテストの実行方法、Sauce LabsやBrowserStackなどのツールとの統合する方法の案内。</td>
  </tr>
 </tbody>
</table>

<h2 id="Selenium" name="Selenium">Selenium</h2>

<p><a href="http://www.seleniumhq.org/">Selenium</a> は最も人気のあるブラウザ自動化ツールです。他の方法もありますが、Selenium を使用する最良の方法は WebDriver を使用することで、強力な API で Selenium 上に構築し、ブラウザを呼び出して自動化し、「このWebページを開く」、「この要素をページ上に移動する」、「このリンクをクリックする」、「リンクがこのURLを開くかどうかを確認する」などといったアクションを実行します。これは、自動テストを実行するのに最適です。<br>
 <br>
 WebDriverのインストール方法と使用方法は、テストの作成と実行に使用するプログラミング環境によって異なります。最も一般的な環境では、WebDriverとその言語、例えばJava、C＃、Ruby、Python、JavaScript（Node）などを使用してWebDriverと通信するのに必要なバインディングをインストールするパッケージまたはフレームワークが利用可能です。異なる言語のSeleniumのセットアップの詳細については、 <a href="http://www.seleniumhq.org/docs/03_webdriver.jsp#setting-up-a-selenium-webdriver-project">Setting Up a Selenium-WebDriver Project</a> を参照してください。<br>
 <br>
 異なるブラウザでは、WebDriverと通信して制御するために異なるドライバが必要です。ブラウザのドライバの入手先などについては、 <a href="http://www.seleniumhq.org/about/platforms.jsp">Platforms Supported by Selenium</a> を参照してください。<br>
 <br>
 Node.jsを使用したSeleniumテストの作成と実行については、始める前にすばやく簡単に行うことができ、フロントエンド開発者にはもっと使い慣れた環境を提供する予定です。</p>

<div class="note">
<p><strong>注</strong>: 他のサーバーサイド環境でWebDriverを使用する方法を知りたい場合は、<a href="http://www.seleniumhq.org/about/platforms.jsp">Platforms Supported by Selenium</a>もチェックしてください。</p>
</div>

<h3 id="Setting_up_Selenium_in_Node" name="Setting_up_Selenium_in_Node">Node で Selenium のセットアップ</h3>

<ol>
 <li>まず、最後の章の <a href="/ja/docs/Learn/Tools_and_testing/Cross_browser_testing/Automated_testing#Setting_up_Node_and_npm">Setting up Node and npm</a> で説明しているように、新しいnpmプロジェクトをセットアップします。<code>selenium-test</code>のように違うものを呼んでください。</li>
 <li>次に私たちはNodeの内部からSeleniumが機能するようにフレームワークをインストールする必要があります。 更新頻度が高く、よく改善されるため、私たちは<a href="https://www.npmjs.com/package/selenium-webdriver">selenium-webdriver</a>を選択します。もしも他の選択をするならば<a href="http://webdriver.io/">webdriver.io</a> と <a href="http://nightwatchjs.org/">nightwatch.js</a> もいい選択です。selenium-webdriverをインストールするため, プロジェクトフォルダの下で以下のコマンドを走らせます:</li>
 <li>
  <pre class="brush: bash"><code>npm install selenium-webdriver</code></pre>
 </li>
</ol>

<div class="note">
<p><strong>注</strong>: 以前に selenium-webdriver をインストールしてブラウザドライバをダウンロードした場合でも、これらの手順を実行することをお勧めします。すべてが最新であることを確認する必要があります。</p>
</div>

<p>Next, you need to download the relevant drivers to allow WebDriver to control the browsers you want to test. You can find details of where to get them from on the <a href="https://www.npmjs.com/package/selenium-webdriver">selenium-webdriver</a> page (see the table in the first section.) Obviously, some of the browsers are OS-specific, but we're going to stick with Firefox and Chrome, as they are available across all the main OSes.</p>

<ol>
 <li>Download the latest <a href="https://github.com/mozilla/geckodriver/releases/">GeckoDriver</a> (for Firefox) and <a href="http://chromedriver.storage.googleapis.com/index.html">ChromeDriver</a> drivers.</li>
 <li>Unpack them into somewhere fairly easy to navigate to, like the root of your home user directory.</li>
 <li>Add the <code>chromedriver</code> and <code>geckodriver</code> driver's location to your system <code>PATH</code> variable. This should be an absolute path from the root of your hard disk, to the directory containing the drivers. 例えば、if we were using a Mac OS X machine, our user name was bob, and we put our drivers in the root of our home folder, the path would be <code>/Users/bob</code>.</li>
</ol>

<div class="note">
<p><strong>注</strong>: Just to reiterate, the path you add to <code>PATH</code> needs to be the path to the directory containing the drivers, not the paths to the drivers themselves! This is a common mistake.</p>
</div>

<p>To set your <code>PATH</code> variable on Mac OS X/most Linux systems:</p>

<ol>
 <li>Open your <code>.bash_profile</code> (or <code>.bashrc</code>) file (if you can't see hidden files, you'll need to display them, see <a href="http://ianlunn.co.uk/articles/quickly-showhide-hidden-files-mac-os-x-mavericks/">Show/Hide hidden files in Mac OS X</a> or <a href="http://askubuntu.com/questions/470837/how-to-show-hidden-folders-in-ubuntu-14-04">Show hidden folders in Ubuntu</a>).</li>
 <li>Paste the following into the bottom of your file (updating the path as it actually is on your machine):
  <pre class="brush: bash">#Add WebDriver browser drivers to PATH

export PATH=$PATH:/Users/bob</pre>
 </li>
 <li>Save and close this file, then restart your Terminal/command prompt to reapply your Bash configuration.</li>
 <li>Check that your new paths are in the <code>PATH</code> variable by entering the following into your terminal:
  <pre class="brush: bash">echo $PATH</pre>
 </li>
 <li>You should see it printed out in the terminal.</li>
</ol>

<p>To set your <code>PATH</code> variable on Windows, follow the instructions at <a href="http://windowsitpro.com/systems-management/how-can-i-add-new-folder-my-system-path">How can I add a new folder to my system path?</a></p>

<p>OK, let's try a quick test to make sure everything is working.</p>

<ol>
 <li>Create a new file inside your project directory called <code>google_test.js</code>:</li>
 <li>Give it the following contents, then save it:
  <pre class="brush: js">var webdriver = require('selenium-webdriver'),
    By = webdriver.By,
    until = webdriver.until;

var driver = new webdriver.Builder()
    .forBrowser('firefox')
    .build();

driver.get('http://www.google.com');

driver.findElement(By.name('q')).sendKeys('webdriver');

driver.sleep(1000).then(function() {
  driver.findElement(By.name('q')).sendKeys(webdriver.Key.TAB);
});

driver.findElement(By.name('btnK')).click();

driver.sleep(2000).then(function() {
  driver.getTitle().then(function(title) {
    if(title === 'webdriver - Google Search') {
      console.log('Test passed');
    } else {
      console.log('Test failed');
    }
  });
});

driver.quit();</pre>
 </li>
 <li>In terminal, make sure you are inside your project folder, then enter the following command:
  <pre class="brush: bash">node google_test</pre>
 </li>
</ol>

<p>You should see an instance of Firefox automatically open up! Google should automatically be loaded in a tab, "webdriver" should be entered in the search box, and the search button will be clicked. WebDriver will then wait for 2 seconds; the document title is then accessed, and if it is "webdriver - Google Search", we will return a message to claim the test is passed. WebDriver will then close down the Firefox instance and stop.</p>

<h2 id="Testing_in_multiple_browsers_at_once" name="Testing_in_multiple_browsers_at_once">一度に複数ブラウザでテストする</h2>

<p>There is also nothing to stop you running the test on multiple browsers simulataneously. Let's try this!</p>

<ol>
 <li>Create another new file inside your project directory called <code>google_test_multiple.js</code>. You can feel free to change the references to some of the other browsers we added, remove them, etc., depending on what browsers you have available to test on your operating system. You'll need to make sure you have the right browser drivers set up on your system. In terms of what string to use inside the <code>.forBrowser()</code> method for other browsers,  see the <a href="http://seleniumhq.github.io/selenium/docs/api/javascript/module/selenium-webdriver/index_exports_Browser.html">Browser enum</a> reference page.</li>
 <li>Give it the following contents, then save it:
  <pre class="brush: js">var webdriver = require('selenium-webdriver'),
    By = webdriver.By,
    until = webdriver.until;

var driver_fx = new webdriver.Builder()
    .forBrowser('firefox')
    .build();

var driver_chr = new webdriver.Builder()
    .forBrowser('chrome')
    .build();

searchTest(driver_fx);
searchTest(driver_chr);

function searchTest(driver) {
  driver.get('http://www.google.com');
  driver.findElement(By.name('q')).sendKeys('webdriver');

  driver.sleep(1000).then(function() {
    driver.findElement(By.name('q')).sendKeys(webdriver.Key.TAB);
  });

  driver.findElement(By.name('btnK')).click();

  driver.sleep(2000).then(function() {
    driver.getTitle().then(function(title) {
      if(title === 'webdriver - Google Search') {
        console.log('Test passed');
      } else {
        console.log('Test failed');
      }
    });
  });

  driver.quit();
}</pre>
 </li>
 <li>In terminal, make sure you are inside your project folder, then enter the following command:
  <pre class="brush: bash">node google_test_multiple</pre>
 </li>
 <li>If you are using a Mac and do decide to test Safari, you might get an error message along the lines of "Could not create a session: You must enable the 'Allow Remote Automation' option in Safari's Develop menu to control Safari via WebDriver." If you get this, follow the given instruction and try again.</li>
</ol>

<p>So here we've done the test as before, except that this time we've wrapped it inside a function, <code>searchTest()</code>. We've created new browser instances for multiple browsers, then passed each one to the function so the test is performed on all three browsers!</p>

<p>Fun huh? Let's move on, look at the basics of WebDriver syntax, in a bit more detail.</p>

<h2 id="WebDriver_syntax_crash_course" name="WebDriver_syntax_crash_course">WebDriver 構文クラッシュコース</h2>

<p>Let's have a look at a few key features of the webdriver syntax. For more complete details, you should consult the <a href="http://seleniumhq.github.io/selenium/docs/api/javascript/module/selenium-webdriver/">selenium-webdriver JavaScript API reference</a> for a detailed reference, and the Selenium main documentation's <a href="http://www.seleniumhq.org/docs/03_webdriver.jsp">Selenium WebDriver</a> and <a href="http://www.seleniumhq.org/docs/04_webdriver_advanced.jsp">WebDriver: Advanced Usage</a> pages, which contain multiple examples to learn from written in different languages.</p>

<h3 id="Starting_a_new_test" name="Starting_a_new_test">新しいテストを始める</h3>

<p>To start up a new test, you need to include the <code>selenium-webdriver</code> module like this:</p>

<pre class="brush: js">var webdriver = require('selenium-webdriver'),
    By = webdriver.By,
    until = webdriver.until;</pre>

<p>Next, you need to create a new instance of a driver, using the <code>new webdriver.Builder()</code> constructor. This needs to have the <code>forBrowser()</code> method chained onto it to specify what browser you want to test with this builder, and the <code>build()</code> method to actually build it (see the <a href="http://seleniumhq.github.io/selenium/docs/api/javascript/module/selenium-webdriver/index_exports_Builder.html">Builder class reference</a> for detailed information on these features).</p>

<pre class="brush: js">var driver = new webdriver.Builder()
    .forBrowser('firefox')
    .build();</pre>

<p>Note that it is possible to set specific configuration options for browsers to be tested, 例えば、you can set a specific version and OS to test in the <code>forBrowser()</code> method:</p>

<pre class="brush: js">var driver = new webdriver.Builder()
    .forBrowser('firefox', '46', 'MAC')
    .build();</pre>

<p>You could also set these options using an environment variable, 例えば、:</p>

<pre class="brush: bash"><code>SELENIUM_BROWSER=firefox:46:MAC</code></pre>

<p>Let's create a new test to allow us to explore this code as we talk about it. Inside your selenium test project directory, create a new file called <code>quick_test.js</code>, and add the following code to it:</p>

<pre class="brush: js">var webdriver = require('selenium-webdriver'),
    By = webdriver.By,
    until = webdriver.until;

var driver = new webdriver.Builder()
    .forBrowser('firefox')
    .build();</pre>

<h3 id="Getting_the_document_you_want_to_test" name="Getting_the_document_you_want_to_test">テストするドキュメントの取得</h3>

<p>To load the page you actually want to test, you use the <code>get()</code> method of the driver instance you created earlier, 例えば、:</p>

<pre class="brush: js"><span class="nx">driver</span><span class="p">.</span><span class="nx">get</span><span class="p">(</span><span class="s1">'http://www.google.com'</span><span class="p">);</span></pre>

<div class="note">
<p><span class="p"><strong>注</strong>: See the <a href="http://seleniumhq.github.io/selenium/docs/api/javascript/module/selenium-webdriver/lib/webdriver_exports_WebDriver.html">WebDriver class reference</a> for details of the features in this section and the ones below it.</span></p>
</div>

<p>You can use any URL to point to your resource, including a <code>file://</code> URL to test a local document:</p>

<pre class="brush: js">driver.get('file:///Users/chrismills/git/learning-area/tools-testing/cross-browser-testing/accessibility/fake-div-buttons.html');</pre>

<p>or</p>

<pre class="brush: js">driver.get('http://localhost:8888/fake-div-buttons.html');</pre>

<p>But it is better to use a remote server location so the code is more flexible — when you start using a remote server to run your tests (see later on), your code will break if you try to use local paths. </p>

<p>Add this line to the bottom of <code>quick_test.js</code> now:</p>

<pre class="brush: js">driver.get('http://mdn.github.io/learning-area/tools-testing/cross-browser-testing/accessibility/native-keyboard-accessibility.html');</pre>

<h3 id="Interacting_with_the_document" name="Interacting_with_the_document">文書とのやりとり</h3>

<p>Now we've got a document to test, we need to interact with it in some way, which usually involves first selecting a specific element to test something about. You can <a href="http://www.seleniumhq.org/docs/03_webdriver.jsp#locating-ui-elements-webelements">select UI elements in many ways</a> in WebDriver, including by ID, class, element name, etc. The actual selection is done by the <code>findElement()</code> method, which accepts as a parameter a selection method. 例えば、to select an element by ID:</p>

<pre class="brush: js"><span class="kd">var</span> <span class="nx">element</span> <span class="o">=</span> <span class="nx">driver</span><span class="p">.</span><span class="nx">findElement</span><span class="p">(</span><span class="nx">By</span><span class="p">.</span><span class="nx">id</span><span class="p">(</span><span class="s1">'myElementId'</span><span class="p">));</span></pre>

<p>One of the most useful ways to find an element by CSS — the  By.css method allows you to select an element using a CSS selector</p>

<p>Enter the following at the bottom of your <code>quick_test.js</code> code now:</p>

<pre class="brush: js"><span class="kd">var</span> <span class="nx">button</span> <span class="o">=</span> <span class="nx">driver</span><span class="p">.</span><span class="nx">findElement</span><span class="p">(</span><span class="nx">By</span><span class="p">.</span><span class="nx">css</span><span class="p">(</span><span class="s1">'</span>button:nth-of-type(1)<span class="s1">'</span><span class="p">));</span></pre>

<h3 id="Testing_your_element" name="Testing_your_element"><span class="p">要素のテスト</span></h3>

<p><span class="p">There are many ways to interact with your web documents and elements on them. You can see useful common examples starting at <a href="http://www.seleniumhq.org/docs/03_webdriver.jsp#getting-text-values">Getting text values</a> on the </span>WebDriver docs.</p>

<p>If we wanted to get the text inside our button, we could do this:</p>

<pre class="brush: js">button.getText().then(function(text) {
  console.log('Button text is \'' + text + '\'');
});</pre>

<p>Add this to <code>quick_test.js</code> now.</p>

<p><span class="p">Making sure you are inside your project directory, try running the test:</span></p>

<pre class="brush: bash"><span class="p">node quick_test.js</span></pre>

<p><span class="p">You should see the button's text label reported inside the console.</span></p>

<p><span class="p">let's do something a bit more useful. delete the previous code entry, then add this line at the bottom instead:</span></p>

<pre class="brush: js">button.click();</pre>

<p>Try running your test again; the button will be clicked, and the <code>alert()</code> popup should appear. At least we know the button is working!</p>

<p>You can interact with the popup too. Add the following to the bottom of the code, and try testing it again:</p>

<pre class="brush: js">var alert = driver.switchTo().alert();

alert.getText().then(function(text) {
  console.log('Alert text is \'' + text + '\'');
});

alert.accept();</pre>

<p>Next, let's try entering some text into one of the form elements. Add the following code and try running your test again:</p>

<pre class="brush: js"><span class="kd">var</span> input <span class="o">=</span> <span class="nx">driver</span><span class="p">.</span><span class="nx">findElement</span><span class="p">(</span><span class="nx">By</span><span class="p">.</span><span class="nx">id</span><span class="p">(</span><span class="s1">'input1'</span><span class="p">));
input.</span>sendKeys('Filling in my form');</pre>

<p>You can submit key presses that can't be represented by normal characters using properties of the <code>webdriver.Key</code> object. 例えば、above we used this construct to tab out of the form input before submitting it:</p>

<pre class="brush: js">driver.sleep(1000).then(function() {
  driver.findElement(By.name('q')).sendKeys(webdriver.Key.TAB);
});
</pre>

<h3 id="Waiting_for_something_to_complete" name="Waiting_for_something_to_complete">何かが完了するのを待つ</h3>

<p>There are times where you'll want to make WebDriver wait for something to complete before carrying on. 例えば、if you load a new page, you'll want to wait for the page's DOM to finish loading before you try to interact with any of its elements, otherwise the test will likely fail.</p>

<p>In our <code>google_test.js</code> test 例えば、we included this block:</p>

<pre class="brush: js">driver.sleep(2000).then(function() {
  driver.getTitle().then(function(title) {
    if(title === 'webdriver - Google Search') {
      console.log('Test passed');
    } else {
      console.log('Test failed');
    }
  });
});</pre>

<p>The <code>sleep()</code> method accepts a value that specifies the time to wait in milliseconds — the method returns a promise that resolves at the end of that time, at which point the code inside the <code>then()</code> executes. In this case we get the title of the current page with the <code>getTitle()</code> method, then return a pass or fail message depending on what its value is.</p>

<p>We could add a <code>sleep()</code> method to our <code>quick_test.js</code> test too — try wrapping your last line of code in a block like this:</p>

<pre class="brush: js">driver.sleep(2000).then(function() {
  input.sendKeys('Filling in my form');
  input.getAttribute("value").then(function(value) {
    if(value !== '') {
      console.log('Form input editable');
    }
  });
});</pre>

<p>WebDriver will now wait for 2 seconds before filling in the form field. We then test whether its value got filled in (i.e. is not empty) by using <code>getAttribute()</code> to retrieve it's <code>value</code> attribute value, and print a message to the console if it is not empty.</p>

<div class="note">
<p><strong>注</strong>: There is also a method called <code><a href="http://seleniumhq.github.io/selenium/docs/api/javascript/module/selenium-webdriver/lib/webdriver_exports_WebDriver.html#wait">wait()</a></code>, which repeatedly tests a condition for a certain length of time, and then carries on executing the code. This also makes use of the <a href="https://seleniumhq.github.io/selenium/docs/api/javascript/module/selenium-webdriver/lib/until.html">util library</a>, which defines common conditions to use along with <code>wait()</code>.</p>
</div>

<h3 id="Shutting_down_drivers_after_use" name="Shutting_down_drivers_after_use">使用後のドライバのシャットダウン</h3>

<p>After you've finished running a test, you should shut down any dirver instances you've opened, to make sure that you don't end up with loads of rogue browser instances open on your machine! This is done using the <code>quit()</code> method. Simply call this on your driver instance when you are finished with it. Add this line to the bottom of your <code>quick_test.js</code> test now:</p>

<pre class="brush: js"><span class="nx">driver</span><span class="p">.</span><span class="nx">quit</span><span class="p">();</span></pre>

<p><span class="p">When you run it, you should now see the test execute and the browser instance shut down again after the text is complete. This is useful for not cluttering up your computer with loads of browser instances, especially if you have so many that it is causing the computer to slow down.</span></p>

<h2 id="Test_best_practices" name="Test_best_practices">テストのベストプラクティス</h2>

<p>There has been a lot written about best practices for writing tests. You can find some good background information at <a href="http://www.seleniumhq.org/docs/06_test_design_considerations.jsp">Test Design Considerations</a>. In general, you should make sure that your tests are:</p>

<ol>
 <li>Using good locator strategies: When you are <a href="#interacting_with_the_document">Interacting with the document</a>, make sure that you use locators and page objects that are unlikely to change — if you have a testable element that you want to perform a test on, make sure that it has a stable ID, or position on the page that can be selected using a CSS selector, which isn't going to just change with the next site iteration. You want to make your tests as non-brittle as possible, i.e. they won't just break when something changes.</li>
 <li>Write atomic tests: Each test should test one thing only, making it easy to keep track of what test file is testing which criterion. As an example, the <code>google_test.js</code> test we looked at above is pretty good, as it just tests a single thing — whether the title of a search results page is set correctly. We could work on giving it a better name so it is easier to work out what it does if we add more google tests. Perhaps <code>results_page_title_set_correctly.js</code> would be slightly better?</li>
 <li>Write autonomous tests: Each test should work on it's own, and not depend on other tests to work.</li>
</ol>

<p>In addition, we should mention test results/reporting — we've been reporting results in our above examples using simple <code>console.log()</code> statements, but this is all done in JavaScript, so you can use whatever test running and resorting system you want, be it <a href="https://mochajs.org/">Mocha</a>/<a href="http://chaijs.com/">Chai</a>/some other kind of combination.</p>

<ol>
 <li>例えば、try making a local copy of our <code><a href="https://github.com/mdn/learning-area/blob/master/tools-testing/cross-browser-testing/selenium/mocha_test.js">mocha_test.js</a></code> example inside your project directory. Put it inside a subfolder called <code>test</code>. This example uses a long chain of promises to run all the steps required in our test — the promise-based methods WebDriver uses need to resolve for it to work properly.</li>
 <li>Install the mocha test harness by running the following command inside your project directory:
  <pre class="brush: bash">npm install --save-dev mocha</pre>
 </li>
 <li>you can now run the test (and any others you put inside your <code>test</code> directory) using the following command:
  <pre class="brush: bash">mocha --no-timeouts</pre>
 </li>
 <li>You should include the <code>--no-timeouts</code> flag to make sure your tests don't end up failing because of Mocha's arbitrary timeout (which is 3 seconds).</li>
</ol>

<div class="note">
<p><strong>注</strong>: <a href="https://github.com/saucelabs-sample-test-frameworks">saucelabs-sample-test-frameworks</a> contains several useful examples showing how to set up different combinations of test/assertion tools.</p>
</div>

<h2 id="Running_remote_tests" name="Running_remote_tests">リモートテストの実行</h2>

<p>It turns out that running tests on remote servers isn't that much more difficult than running them locally. You just need to create your driver instance, but with a few more features specified, including the capabilities of the browser you want to test on, the address of the server, and the user credentials you need (if any) to access it.</p>

<h3 id="BrowserStack" name="BrowserStack">BrowserStack</h3>

<p>Getting Selenium tests to run remotely on BrowserStack is easy. The code you need should follow the pattern seen below.</p>

<p>Let's write an example:</p>

<ol>
 <li>Inside your project directory, create a new file called <code>bstack_google_test.js</code>.</li>
 <li>Give it the following contents:
  <pre class="brush: js">var webdriver = require('selenium-webdriver'),
    By = webdriver.By,
    until = webdriver.until;

// Input capabilities
var capabilities = {
   'browserName' : 'Firefox',
   'browser_version' : '56.0 beta',
   'os' : 'OS X',
   'os_version' : 'Sierra',
   'resolution' : '1280x1024',
   'browserstack.user' : '<code>YOUR-USER-NAME</code>',
   'browserstack.key' : '<code>YOUR-ACCESS-KEY</code>',
   'browserstack.debug' : 'true',
   'build' : 'First build'
};

var driver = new webdriver.Builder().
  usingServer('http://hub-cloud.browserstack.com/wd/hub').
  withCapabilities(capabilities).
  build();

driver.get('http://www.google.com');
driver.findElement(By.name('q')).sendKeys('webdriver');

driver.sleep(1000).then(function() {
  driver.findElement(By.name('q')).sendKeys(webdriver.Key.TAB);
});

driver.findElement(By.name('btnK')).click();

driver.sleep(2000).then(function() {
  driver.getTitle().then(function(title) {
    if(title === 'webdriver - Google Search') {
      console.log('Test passed');
    } else {
      console.log('Test failed');
    }
  });
});

driver.quit();</pre>
 </li>
 <li>From your <a href="https://www.browserstack.com/automate">BrowserStack automation dashboard</a>, get your user name and access key (see <em>Username and Access Keys</em>). Replace the <code>YOUR-USER-NAME</code> and <code>YOUR-ACCESS-KEY</code> placeholders in the code with your actual user name and access key values (and make sure you keep them secure).</li>
 <li>Run your test with the following command:
  <pre class="brush: bash">node bstack_google_test</pre>
  The test will be sent to BrowserStack, and the test result will be returned to your console. This shows the importance of including some kind of result reporting mechanism!</li>
 <li>Now if you go back to the <a href="https://www.browserstack.com/automate">BrowserStack automation dashboard</a> page, you'll see your test listed:<br>
  <img alt="" src="https://mdn.mozillademos.org/files/15383/bstack_automated_results.png" style="border-style: solid; border-width: 1px; display: block; height: 189px; margin: 0px auto; width: 700px;"></li>
</ol>

<p>If you click on the link for your test, you'll get to a new screen where you will be able to see a video recording of the test, and multiple detailed logs of information pertaining to it.</p>

<div class="note">
<p><strong>注</strong>: The <em>Resources</em> menu option on the Browserstack automation dashboard contains a wealth of useful information on using it to run automated tests. See <a href="https://www.browserstack.com/automate/node">Node JS Documentation for writing automate test scripts in Node JS</a> for the node-specific information. Expore the docs to find out all the useful things BrowserStack can do.</p>
</div>

<div class="note">
<p><strong>注</strong>: If you don't want to write out the capabilities objects for your tests by hand, you can generate them using the generators embedded in the docs. See <a href="https://www.browserstack.com/automate/node#run-tests-on-mobile">Run tests on mobile browsers</a> and <a href="https://www.browserstack.com/automate/node#setting-os-and-browser">Run tests on desktop browsers</a>.</p>
</div>

<h4 id="Filling_in_BrowserStack_test_details_programmatically" name="Filling_in_BrowserStack_test_details_programmatically">プログラムによるBrowserStackテストの詳細の入力</h4>

<p>You can use the BrowserStack REST API and some other capabilities to annotate your test with more details, such as whether it passed, why it passed, what project the test is part of, etc. BrowserStack doesn't know these details 既定では!</p>

<p>Let's update our <code>bstack_google_test.js</code> demo, to show how these features work:</p>

<ol>
 <li>First, we 'll need to import the node request module, so we can use it to send requests to the REST API. Add the following line at the very top of your code:
  <pre class="brush: js">var request = require("request");</pre>
 </li>
 <li>Now we'll update our <code>capabilities</code> object to include a project name — add the following line before the closing curly brace, remembering to add a comma at the end of the previous line (you can vary the build and project names to organize the tests in different windows in the BrowserStack automation dashboard):
  <pre class="brush: js">'project' : 'Google test 2'</pre>
 </li>
 <li>Next we need to access the <code>sessionId</code> of the current session, so we know where to send the request (the ID is included in the request URL, as you'll see later). Include the following lines just below the block that creates the <code>driver</code> object (<code>var driver ...</code>) :
  <pre class="brush: js">var sessionId;

driver.session_.then(function(sessionData) {
    sessionId = sessionData.id_;
});</pre>
 </li>
 <li>Finally, update the <code>driver.sleep(2000)</code> ... block near the bottom of the code to add REST API calls (again, replace the <code>YOUR-USER-NAME</code> and <code>YOUR-ACCESS-KEY</code> placeholders in the code with your actual user name and access key values):
  <pre class="brush: js">driver.sleep(2000).then(function() {
  driver.getTitle().then(function(title) {
    if(title === 'webdriver - Google Search') {
      console.log('Test passed');
      request({uri: "https://<code>YOUR-USER-NAME</code>:<code>YOUR-ACCESS-KEY</code>@www.browserstack.com/automate/sessions/" + sessionId + ".json", method:"PUT", form:{"status":"passed","reason":"Google results showed correct title"}});
    } else {
      console.log('Test failed');
      request({uri: "https://<code>YOUR-USER-NAME</code>:<code>YOUR-ACCESS-KEY</code>@www.browserstack.com/automate/sessions/" + sessionId + ".json", method:"PUT", form:{"status":"failed","reason":"Google results showed wrong title"}});
    }
  });
});</pre>
 </li>
</ol>

<p>These are fairly intuitive — once the test completes, we send an API call to BrowserStack to update the test with a passed or failed status, and a reason for the result.</p>

<p>If you now go back to your <a href="https://www.browserstack.com/automate">BrowserStack automation dashboard</a> page, you should see your test session available, as before, but with the updated data attached to it:</p>

<p><img alt="" src="https://mdn.mozillademos.org/files/15385/bstack_custom_results.png" style="border-style: solid; border-width: 1px; display: block; margin: 0px auto;"></p>

<h3 id="Sauce_Labs" name="Sauce_Labs">Sauce Labs</h3>

<p>Getting Selenium tests to run remotely on Sauce Labs is also very simple, and very similar to BrowserStack albeit with a few syntactic differences. The code you need should follow the pattern seen below.</p>

<p>Let's write an example:</p>

<ol>
 <li>Inside your project directory, create a new file called <code>sauce_google_test.js</code>.</li>
 <li>Give it the following contents:
  <pre class="brush: js">var webdriver = require('selenium-webdriver'),
    By = webdriver.By,
    until = webdriver.until,
    username = "YOUR-USER-NAME",
    accessKey = "YOUR-ACCESS-KEY";

var driver = new webdriver.Builder()
    .withCapabilities({
      'browserName': 'chrome',
      'platform': 'Windows XP',
      'version': '43.0',
      'username': username,
      'accessKey': accessKey
    })
    .usingServer("https://" + username + ":" + accessKey +
          "@ondemand.saucelabs.com:443/wd/hub")
    .build();

driver.get('http://www.google.com');

driver.findElement(By.name('q')).sendKeys('webdriver');

driver.sleep(1000).then(function() {
  driver.findElement(By.name('q')).sendKeys(webdriver.Key.TAB);
});

driver.findElement(By.name('btnK')).click();

driver.sleep(2000).then(function() {
  driver.getTitle().then(function(title) {
    if(title === 'webdriver - Google Search') {
      console.log('Test passed');
    } else {
      console.log('Test failed');
    }
  });
});

driver.quit();</pre>
 </li>
 <li>From your <a href="https://saucelabs.com/beta/user-settings">Sauce Labs user settings</a>, get your user name and access key. Replace the <code>YOUR-USER-NAME</code> and <code>YOUR-ACCESS-KEY</code> placeholders in the code with your actual user name and access key values (and make sure you keep them secure).</li>
 <li>Run your test with the following command:
  <pre class="brush: bash">node sauce_google_test</pre>
  The test will be sent to Sauce Labs, and the test result will be returned to your console. This shows the importance of including some kind of result reporting mechanism!</li>
 <li>Now if you go to your <a href="https://saucelabs.com/beta/dashboard/tests">Sauce Labs Automated Test dashboard</a> page, you'll see your test listed; from here you'll be able to see videos, screenshots, and other such data.<br>
  <img alt="" src="https://mdn.mozillademos.org/files/14235/sauce_labs_automated_test.png" style="border-style: solid; border-width: 1px; display: block; margin: 0px auto;"></li>
</ol>

<div class="note">
<p><strong>注</strong>: Sauce Labs' <a href="https://wiki.saucelabs.com/display/DOCS/Platform+Configurator/#/">Platform Configurator</a> is a useful tool for generating capability objects to feed to your driver instances, based on what browser/OS you want to test on.</p>
</div>

<div class="note">
<p><strong>注</strong>: for more useful details on testing with Sauce Labs and Selenium, check out <a href="https://wiki.saucelabs.com/display/DOCS/Getting+Started+with+Selenium+for+Automated+Website+Testing">Getting Started with Selenium for Automated Website Testing</a>, and <a href="https://wiki.saucelabs.com/display/DOCS/Instant+Selenium+Node.js+Tests">Instant Selenium Node.js Tests</a>.</p>
</div>

<h4 id="Filling_in_Sauce_Labs_test_details_programmatically" name="Filling_in_Sauce_Labs_test_details_programmatically">Sauce Labsテストの詳細をプログラムで書き込む</h4>

<p>You can use the Sauce Labs API to annotate your test with more details, such as whether it passed, the name of the test, etc. Sauce Labs doesn't know these details 既定では!</p>

<p>To do this, you need to:</p>

<ol>
 <li>Install the Node Sauce Labs wrapper using the following command (if you've not already done it for this project):
  <pre class="brush: bash"><code class="language-html">npm install saucelabs --save-dev</code></pre>
 </li>
 <li>Require saucelabs — put this at the top of your <code>sauce_google_test.js</code> file, just below the previous variable declarations:
  <pre class="brush: js line-numbers  language-js"><code class="language-js"><span class="keyword token">var</span> SauceLabs <span class="operator token">=</span> <span class="function token">require</span><span class="punctuation token">(</span><span class="string token">'saucelabs'</span><span class="punctuation token">)</span><span class="punctuation token">;</span></code></pre>
 </li>
 <li>Create a new instance of SauceLabs, by adding the following just below that:
  <pre class="brush: js">var saucelabs = new SauceLabs({
    username : "YOUR-USER-NAME",
    password : "YOUR-ACCESS-KEY"
});</pre>
  Again, replace the <code>YOUR-USER-NAME</code> and <code>YOUR-ACCESS-KEY</code> placeholders in the code with your actual user name and access key values (note that the saucelabs npm package rather confusingly uses <code>password</code>, not <code>accessKey</code>). Since you are using these twice now, you may want to create a couple of helper variables to store them in.</li>
 <li>Below the block where you define the <code>driver</code> variable (just below the <code>build()</code> line), add the following block — this gets the correct driver <code>sessionID</code> that we need to write data to the job (you can see it action in the next code block):
  <pre class="brush: js">driver.getSession().then(function (sessionid){
      driver.sessionID = sessionid.id_;
});</pre>
 </li>
 <li>Finally, replace the <code>driver.sleep(2000)</code> ... block near the bottom of the code with the following:
  <pre class="brush: js">driver.sleep(2000).then(function() {
  driver.getTitle().then(function(title) {
    if(title === 'webdriver - Google Search') {
      console.log('Test passed');
      var testPassed = true;
    } else {
      console.log('Test failed');
      var testPassed = false;
    }

    saucelabs.updateJob(driver.sessionID, {
      name: 'Google search results page title test',
      passed: testPassed
    });
  });
});</pre>
 </li>
</ol>

<p>Here we've set a <code>testPassed</code> variable to <code>true</code> or <code>false</code> depending on whether the test passed or fails, then we've used the <code>saucelabs.updateJob()</code> method to update the details.</p>

<p>If you now go back to your <a href="https://saucelabs.com/beta/dashboard/tests">Sauce Labs Automated Test dashboard</a> page, you should see your new job now has the updated data attached to it:</p>

<p><img alt="" src="https://mdn.mozillademos.org/files/14239/sauce_labs_updated_job_info.png" style="border-style: solid; border-width: 1px; display: block; height: 150px; margin: 0px auto; width: 1088px;"></p>

<h3 id="Your_own_remote_server" name="Your_own_remote_server">自身のリモートサーバ</h3>

<p>If you don't want to use a service like Sauce Labs or BrowserStack, you can always set up your own remote testing server. Let's look at how to do this.</p>

<ol>
 <li>The Selenium remote server requires Java to run. Download the latest JDK for your platform from the <a href="http://www.oracle.com/technetwork/java/javase/downloads/index.html">Java SE downloads page</a>. Install it when it is downloaded.</li>
 <li>Next, download the latest <a href="http://selenium-release.storage.googleapis.com/index.html">Selenium standalone server</a> — this acts as a proxy between your script and the browser drivers. Choose the latest stable version number (i.e. not a beta), and from the list choose a file starting with "selenium-server-standalone". When this has downloaded, put it in a sensible place, like in your home directory. If you've not already added the location to your <code>PATH</code>, do so now (see the <a href="#setting_up_selenium_in_node">Setting up Selenium in Node</a> section).</li>
 <li>Run the standalone server by entering the following into a terminal on your server computer
  <pre class="brush: bash"><code>java -jar </code>selenium-server-standalone-3.0.0.jar</pre>
  (update the <code>.jar</code> filename) so it matches exactly what file you've got.</li>
 <li>The server will run on <code><a href="http://localhost:4444/wd/hub">http://localhost:4444/wd/hub</a></code> — try going there now to see what you get.</li>
</ol>

<p>Now we've got the server running, let's create a demo test that will run on the remote selenium server.</p>

<ol>
 <li>Create a copy of your <code>google_test.js</code> file, and call it <code>google_test_remote.js</code>; put it in your project directory.</li>
 <li>Update the second code block (which starts with <code>var driver = </code>) like so
  <pre class="brush: js">var driver = new webdriver.Builder()
    .forBrowser('firefox')
    .usingServer('http://localhost:4444/wd/hub')
    .build();</pre>
 </li>
 <li>Run your test, and you should see it run as expected; this time however you will be runing it on the standalone server:
  <pre class="brush: bash">node google_test_remote.js</pre>
 </li>
</ol>

<p>So this is pretty cool. We have tested this locally, but you could set this up on just about any server along with the relevant browser drivers, and then connect your scripts to it using the URL you choose to expose it at.</p>

<h2 id="Integrating_selenium_with_CI_tools" name="Integrating_selenium_with_CI_tools">selenium と CI ツールのインテグレーション</h2>

<p>As another point, it is also possible to integrate Selenium and related tools like Sauce Labs with continuous integration (CI) tools — this is useful, as it means you can run your tests via a CI tool, and only commit new changes to your code repository if the tests pass.</p>

<p>It is out of scope to look at this area in detail in this article, but we'd suggest getting started with Travis CI — this is probably the easiest CI tool to get started with, and has good integration with web tools like GitHub and Node.</p>

<p>To get started, see 例えば、:</p>

<ul>
 <li><a href="https://docs.travis-ci.com/user/for-beginners">Travis CI for complete beginners</a></li>
 <li><a href="https://docs.travis-ci.com/user/languages/javascript-with-nodejs/">Building a Node.js project</a> (with Travis)</li>
 <li><a href="https://docs.travis-ci.com/user/sauce-connect/">Using Sauce Labs with Travis CI</a></li>
</ul>

<h2 id="Summary" name="Summary">まとめ</h2>

<p>This module should have proven fun, and should have given you enough of an insight into writing and running automated tests for you to get going with writing your own automated tests.</p>

<p>{{PreviousMenu("Learn/Tools_and_testing/Cross_browser_testing/Automated_testing", "Learn/Tools_and_testing/Cross_browser_testing")}}</p>



<h2 id="In_this_module" name="In_this_module">このモジュール内</h2>

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



<div id="simple-translate-button" class="hidden"></div>

<div id="simple-translate-panel" class="hidden">
<p>...</p>
</div>

<div id="simple-translate-button"></div>

<div id="simple-translate-panel">
<p>...</p>
</div>