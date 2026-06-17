<p align="center">
<img src="https://i.imgur.com/Clzj7Xs.png" alt="osTicket logo"/>
</p>

<h1>osTicket installation</h1>
<p>This tutorial outlines the prerequisites and installation of an open-source help desk ticketing system, namely osTicket.</p>
<p>It also contains a simple explanation of what each component is and why it is necessary to install it for osTicket to function correctly.</p>

<h2>Environments and Technologies Used</h2>

- Microsoft Azure (Virtual Machines/Compute)
- Remote Desktop
- Internet Information Services (IIS)
- osTicket

<h2>List of Prerequisites</h2>

- To have a VM or physical computer where you want to install osTicket

<h2>High-Level Deployment and Configuration Steps</h2>

- Download the necessary installation files

<h2>Installation process</h2>

<h3>Downloading the necessary files</h3>
<p>To install osTicket, we need to download the necessary files to our VM/computer. I'm currently using a VM, so I'll use our VM going forward. For this purpose, we could either go and search for them ourselves or download a zip. Even though downloading the zip may seem faster and easier, it carries a security risk because we don't know whether the files in the zip have been tampered with. Given our situation, I think it's best to go ahead and search for the files manually. Nevertheless, to make the process easier, I have compiled a little table with all the requirements and the original links to each one of them</p>

|Requierment   |Original download link   |
|---|---|
|PHP Programing language |https://downloads.php.net/~windows/releases/archives/php-7.3.8-nts-Win32-VC15-x86.zip   |
|PHP Manager for IIS |https://github.com/RonaldCarter/PHPManager/releases/download/V1.5.0/PHPManagerForIIS_V1.5.0.msi   |
|URL Rewrite Installer   |https://download.microsoft.com/download/1/2/8/128E2E22-C1B9-44A4-BE2A-5859ED1D4592/rewrite_amd64_en-US.msi   |
|Microsoft Visual C++ Redistributable (2015-2022)   |https://download.visualstudio.microsoft.com/download/pr/40b59c73-1480-4caf-ab5b-4886f176bf71/435A0DE411B991E2BFC7FD1D5439639E7B32206960D3099370E36172018F52FE/VC_redist.x86.exe   |
|osTicket installation files   |https://github.com/osTicket/osTicket/releases/download/v1.15.8/osTicket-v1.15.8.zip   |
|MySQL   |https://downloads.mysql.com/archives/get/p/23/file/mysql-5.5.62-win32.msi   |
|HeidiSQL |https://github.com/HeidiSQL/HeidiSQL/releases/download/12.17/HeidiSQL_12.17.0.7270_Setup.exe   |

> [!IMPORTANT]
>This tutorial is using the 1.15.8 release of osTicket and the 7.3.8 release of PHP. If you wanna use another version of either one, be aware that they may cause some errors with PHP extensions, compatibility, and other parts of the tutorial

<p>You should go requirement by requirement, clicking the links on the table to download it. After you are done, you should have the following files in your download folder.</p>
<img src="https://i.imgur.com/FviBLIz.png"  height="35%" width="35%"/>

<h3>Installing iis</h3>
<p>To start installing osTicket, first, we have to install IIS. </p>
<p>Think about a webpage. For a webpage to run, there needs to be a server somewhere that hosts it. We can think of a server pretty much just as someone else's computer.</p>
<p>With the help of IIS, we can give our VM the capacity to act like a server, that is, to run webpages within itself.</p>
<p>To install IIS, we can either go to <code>turn Windows features on or off</code> or to the server management window, which one you choose depends on your operating system</p>
<img src="https://i.imgur.com/xIrqmPz.png" height="25%" width="25%"/>
<p>Then, we go to the Add Roles and Features option</p>
<img src="https://i.imgur.com/axxeIOb.png" height="25%" width="25%"/>
<p>Once there, we can simply click on the IIS option</p>
<img src="https://i.imgur.com/XdgT50E.png"  height="35%" width="35%"/>
<p>Make sure we also enable the CGI option within the application development toggle</p>
<img src="https://i.imgur.com/D9731Ft.png"  height="35%" width="35%"/>
<p>And then we simply keep clicking next until we get the option to install</p>

<br>
<p><code>127.0.0.1</code> is the loopback address, normally its porpuse its to ping to the VM itself. But after we install our IIS server, we can also use it to browse to the webpage now hosted on our VM by simply typing it in the browser</p>
<p>When you do that, something like this should show up</p>
<img src="https://i.imgur.com/jErzqYk.png"  height="35%" width="35%"/>
<br>

<p>Webpages usually run with code, normally some combination of HTML, CSS, JavaScript </p>
<p>The webpage that we are now self-hosting is no exception; it also runs using code, and we can actually see that code if we go to <code>C:\inetpub\wwwroot</code></p>
<img src="https://i.imgur.com/oCKY3Gx.png"  height="35%" width="35%"/>
<p> Importantly, we can not only observe this code. We can actually modify it to turn our self-hosted webpage into a totally different webpage. And that is exactly what we will be doing to install osTicket</p> 

<h3>PHP configurartion</h3>
<p> PHP is a programming language primarily used in webpage development. It is also the language that osTicket is programmed in; thus, we must install it and configure our IIS server to use it before we can use osTicket</p>

<p>To install PHP, we first have to extract the <code>php zip</code>, which contains all the files necessary for the language to be used</p>
<img src="https://i.imgur.com/HHmJDkN.png"  height="25%" width="25%"/>

<p>Then we need to go to <code>C:\</code> and create a folder called php</p>
<img src="https://i.imgur.com/LFJlr5r.png"  height="25%" width="25%"/>
<p>and then we have to copy all of the files from the php folder we just extracted into our new <code>C:\php</code> folder. Once we copy the files our <code>C:\php</code> folder should look something like this:</p>
<img src="https://i.imgur.com/LFJlr5r.png"  height="25%" width="25%"/>

<br>
<p>Now, how IIS works is that whenever our IIS server needs to read PHP files, it will try to look for some executable that it can use to know how to process said files. Due to the simple fact that we already installed all the files necessary to use PHP on our computer, we know for a fact that those files exist. Only that our IIS server doesn't know where to find them, and as such, it is unable to use them. The process of making our IIS server aware of where to find the PHP files is called PHP version registration</p>
<p>To make the process of registering a new PHP version to our IIS server easier, we need to run <code>php manager for iis</code></p>
<img src="https://i.imgur.com/dGq6emB.png"  height="25%" width="25%"/>


<p>Once that's finished, we can use the Windows search bar to look for the IIS Management Console and click it</p> 
<img src="https://i.imgur.com/gbsuuQc.png"  height="25%" width="25%"/>
<p>This will open our IIS management console, and we should see a <code>php manager</code> icon in the main page</p>
<img src="https://i.imgur.com/PJUlZAY.png"  height="25%" width="25%"/>
<p>that is the option we just created by running <code>php manager for iis</code></p>

<p>To finally register our new PHP version, we need to click the aforementioned icon to move into this page</p>
<img src="https://i.imgur.com/zhqiYhu.png"  height="25%" width="25%"/>
<p>And there we must click the register a new PHP version button, and then we browse to the <code>C:\php</code> folder we created a couple of steps before and simply select <code>php-cgi.exe</code> and click open</p>
<img src="https://i.imgur.com/Rj8P2C3.png"  height="25%" width="25%"/>
<p>After clicking the open button, we should end up with a page looking somewhat like this</p>
<img src="https://i.imgur.com/EZ6NS9G.png"  height="25%" width="25%"/>
<p>Notice how where it says php executable, it is pointing at our <code>php-cgi.exe</code> route</p>

<br>
<p>We still need to install some PHP extensions</p>
<p>PHP extensions are libraries. That is code someone else wrote, that extends the functionality of PHP to meet the needs of specific apps</p>
<p>For instance, osTicket, instead of having a built-in email management system, relies on php_imap.dll, a library that uses the IMAP protocol for managing emails. Because of that, every time osTicket needs to manage emails, it will simply call on php_imap to do the email management work for it</p>
<p>Apps commonly decide to rely on PHP extensions instead of creating a built-in system for the sake of time and efficiency. The downside of relying on PHP extensions is that they must be installed before the application can use them.</p>
<p>I organized the PHP extensions we need to install and what they do in the following table</p>

|PHP extension   |Functionality it provides   |
|---|---|
|php_imap.dll |It's a library that will allow osTicket to use the IMAP protocol, which is a protocol to manage emails   |
|php_intl.dll |It's a library for Internationalization, it makes the app usable on different languages, supports various scripts, etc.    |
|php_opcache.dll   |It's a library that runs precompiled PHP scripts in memory and thus increases performance  |

<p>To install them, we go to the same page we were on previously. We click enable or disable an extension, which should take us to this page</p>
<img src="https://i.imgur.com/6pQWlym.png"  height="25%" width="25%"/>
<p>And then we copy our PHP extension name to the filter field to make our extension appear, and when it appears, we simply right click it and pick enable</p>
<img src="https://i.imgur.com/uQKtOgx.png"  height="25%" width="25%"/>
<p>We repeat that process for the 3 extensions </p>
<p>Once the 3 of them are enabled, we right-click and click back to the main page to go back to the main page</p>
<img src="https://i.imgur.com/NQD1W6t.png"  height="25%" width="25%"/>
<p>When we are back at the main page, we should observe that under the PHP extensions label, it now says there are 11 extensions enabled instead of 8</p>
<img src="https://i.imgur.com/0am6muQ.png"  height="25%" width="25%"/>

<h3>Enable url rewrite</h3>
<p>URL rewrite allows the user, or in this case, osTicket, to configure rules to map any given URL to any other URL</p>
<p>This is better explained with an example. With URL rewrite, we can, for instance, take the URL <code>http://localhost/article/342/some-article-title</code> and configure rules within our IIS server to turn it into <code>http://localhost/article.aspx?id=342&title=some-article-title</code></p>
<p> We need to enable this on our osTicket VM for two reasons. First, osTicket constantly converts URLs, and doing so is essential to its functioning. And secondly, the PHP files mentioned before that osTicket uses to run expect certain specific URLs, and if those URLs are not provided or are provided in a form that is not expected, errors may arise</p>

<p>To enable url rewrite we have to run <code>rewrite_amd64_en-US</code></p>
<img src="https://i.imgur.com/Z2F3sie.png"  height="25%" width="25%"/>

<h3>Microsoft Visual C++ Redistributable</h3>
<p>When you create an application in the C programming language family, it is almost impossible that you don't rely on libraries</p>
<p>libraries can be think of as reusable code, like functions or classes that other people created and that you are reusing in your own aplication, using libraries is really common but it has a downside apps that are build using any specific library will then need that library to run even after the code is compiled into a <code>.exe</code> file</p>
<p>osTicket is affected by that because some components it needs for its correct working really depend on those libraries</p>
<p>For instance, we need to install said libraries to use:</p>
<p><code>certain php extentions</code></p>
<p><code>certain iis modules</code></p>
<p><code>image processing functions within osTicket</code></p>
<p>the specific libraries we need to install are the <code>Visual C++ Runtime libraries</code> and to install them we need to run <code>VC_redist.x86</code></p>
<img src="https://i.imgur.com/xsiCtXa.png"  height="25%" width="25%"/>

<h3>Deploying the webapp</h3>
<p>Now we have everything required for the osTicket webapp to run, but how do we actually run it?</p>
<p>Remember the <code>C:\inetpub\wwwroot</code> folder that i mentioned earlier</p>
<p>Well, if we simply switch those files with the files that define ostciket, we can convert our default webpage into osTicket</p>

<br>
<p>To find the necessary files, we can go to the <code>osTicket files zip</code> and extract its contents by right-clicking and clicking extract all</p>
<img src="https://i.imgur.com/iX3WcID.png"  height="25%" width="25%"/>
<p>Afterwards, we go to the folder that we just created when we extracted the osTicket files, and we browse into the upload folder</p>
<img src="https://i.imgur.com/yL4ppuJ.png"  height="25%" width="25%"/>
<p>We then need to delete the files on the <code>C:\inetpub\wwwroot</code> folder</p>
<img src="https://i.imgur.com/QmmSzUt.png"  height="25%" width="25%"/>
<p>and then we copy the files of the upload folder into it to the <code>C:\inetpub\wwwroot</code> folder</p>
<img src="https://i.imgur.com/zg4vlLL.png"  height="25%" width="25%"/>
<p>After doing all of that to ensure our configurations were made effective, we need to go back to the IIS Manager and click the restart button under the manage server label to restart our IIS server</p>
<img src="https://i.imgur.com/mT0yYF2.png"  height="25%" width="25%"/>

<br>
<p>After we are done doing that, our IIS server should be looking at the files that define osTicket as the source code for our IIS website</p>
<p>To easily test if this is the case, we can browse to <code>127.0.0.1</code> and see </p>
