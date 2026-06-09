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
<p>To install osTicket, we need to download the necessary files to our VM/computer. I'm personally using a VM, so henceforth I will be using our VM. For this purpose, we could either go and search for them ourselves or download a zip, even though the option of downloading the zip may appear faster and easier, it has the drawback of potentially causing security issues due to the simple fact that we don't know if the files on the zip have been tampered with. Given our situation i think its best to go ahead and search for the files manually. Nevertheless, to make the process easier, I have compiled a little table with all the requirements and the original links to each one of them</p>

|Requierment   |Original download link   |
|---|---|
|PHP Programing language |https://downloads.php.net/~windows/releases/archives/php-7.3.8-nts-Win32-VC15-x86.zip   |
|PHP Manager for IIS |https://github.com/RonaldCarter/PHPManager/releases/download/V1.5.0/PHPManagerForIIS_V1.5.0.msi   |
|URL Rewrite Installer   |https://download.microsoft.com/download/1/2/8/128E2E22-C1B9-44A4-BE2A-5859ED1D4592/rewrite_amd64_en-US.msi   |
|Microsoft Visual C++ Redistributable (2015-2022)   |https://download.visualstudio.microsoft.com/download/pr/40b59c73-1480-4caf-ab5b-4886f176bf71/435A0DE411B991E2BFC7FD1D5439639E7B32206960D3099370E36172018F52FE/VC_redist.x86.exe   |
|osTicket installation files   |https://github.com/osTicket/osTicket/releases/download/v1.15.8/osTicket-v1.15.8.zip   |
|mysql (original)   |https://downloads.mysql.com/archives/get/p/23/file/mysql-5.5.62-win32.msi   |
|mysql (the one i just downloaded btw)   |https://dev.mysql.com/downloads/file/?id=523569   |
|heidisql |http://www.heidisql.com/installers/HeidiSQL_12.3.0.6589_Setup.exe   |
|heidisql (new) |https://github.com/HeidiSQL/HeidiSQL/releases/download/12.17/HeidiSQL_12.17.0.7270_Setup.exe   |

<p>You should go requirment by requierment clicking the links on the table to download it. After you are done, you should have the following files in your download folder.</p>
<img src="https://i.imgur.com/f5kHs53.png"  height="35%" width="35%"/>

> [!IMPORTANT]
>This tutorial is using the 1.15.8 realse of osTicket, and the 7.3.8 realease of php, if you wanna use other version of either one be aware that thay may cause some errors with php extentions, compatibility and other parts of the tutorial

<h3>Installing iis</h3>
<p>To start installing osTicket first we have to install iis.</p>
<p>Think about a webpage, for a webpage to run there needs to be a server somewhere that hosts it. We can think of a server pretty much just as someone elses computer.</p>
<p>With the help of iis, we can give our vm the capacity to act like a server, that is to run webpages within itself.</p>
<p>To install iis we can either go to <code>turn windows features on or off</code> or to the add roles and features option on the server managmente window, which one you chose depends on your operating system</p>
<p>Once there we can simply click on the iis option</p>
<img src="https://i.imgur.com/XdgT50E.png"  height="35%" width="35%"/>
<p>Make sure we also enable the cgi option within the aplication development toggle</p>
<img src="https://i.imgur.com/D9731Ft.png"  height="35%" width="35%"/>

<br>
<p><code>127.0.0.1</code> is the loop back address, normally its porpuse its to ping to the vm itself. But after we install our iis server we can also use it to browse to the webpage now hosted on our vm by simplfy typing it in the browser</p>
<p>When you do that something like this should show up</p>
<img src="https://i.imgur.com/jErzqYk.png"  height="35%" width="35%"/>
<br>

<p>Webpages usually run with code, normally some convination of: html, css, javascript</p>
<p>The webpage the we are now selfhosting is no exception it also runs using code and we can actually see that code if we go to <code>C:\inetpub\wwwroot</code></p>
<img src="https://i.imgur.com/oCKY3Gx.png"  height="35%" width="35%"/>
<p>Importantlly, we can not only observe this code. We can actually modify it to turn our self hosted webpage into a totally different webpage. And that is exactly what we will be doing to install osTicket</p> 

<h3>PHP configurartion</h3>
<p>Php is a programing language primarly used in webpage development. It is also the language that osTicket is programmed in; thus, we must install it and configure our iis server to use it before we can use osTicket</p>

<p>To install php we firslly have to extract the <code>php zip</code> which contains all the files necesarry for the language to be used</p>
<img src="https://i.imgur.com/HHmJDkN.png"  height="25%" width="25%"/>

<p>Then we need to go to <code>C:\</code> and create a folder called php</p>
<img src="https://i.imgur.com/LFJlr5r.png"  height="25%" width="25%"/>
<p>and then we have to copy all of the files from the php folder we just extracted into our new <code>C:\php</code> folder. Once we copy the files our <code>C:\php</code> folder should look something like this:</p>
<img src="https://i.imgur.com/LFJlr5r.png"  height="25%" width="25%"/>

<br>
<p>Now how iis works is that whenever our iis server needs to read php files it will try to look for some executable that it can use to know how to processs said files. Due to the simple fact we already intalled all the files necessary to use php into our computer we know for a fact those files exist. Only that our iis server doesnt know where to find them and as such it is unable to use them. The process of making our iis server aware of where to find the php files is called php version registration</p>
<p>To make the process of registering a new php version to our iis server easier we need to run <code>php manager for iis</code></p>
<img src="https://i.imgur.com/dGq6emB.png"  height="25%" width="25%"/>


<p>Once thats finished we can use the windows search bar to look for the iis managmente console and click it</p> 
<img src="https://i.imgur.com/82etMyo.png"  height="25%" width="25%"/>
<p>This will open our iis managment console, and we should see a <code>php manager</code> icon in the main page</p>
<img src="https://i.imgur.com/82etMyo.png"  height="25%" width="25%"/>
<p>that is the option we just created by running <code>php manager for iis</code></p>

<p>To finally register our new php version we need to click the aforementioned icon to move into this page</p>
<img src="https://i.imgur.com/Rj8P2C3.png"  height="25%" width="25%"/>
<p>And there we must click the register a new php version button and then we browser to the <code>C:\php</code> folder we created a couple steps before and simply select <code>php-cgi.exe</code> and click open</p>
<img src="https://i.imgur.com/Rj8P2C3.png"  height="25%" width="25%"/>



<h3>Enable url rewrite</h3>
<p>Url re write allows the user or in this case osTicket to configure rules to map any given url to any other url</p>
<p>this is better explained with an example, with url rewrite we can for instance take the url <code>http://localhost/article/342/some-article-title</code> and configure rules within our iis server to turn it into <code>http://localhost/article.aspx?id=342&title=some-article-title</code></p>
<p>we need to enable this on our osTicket VM because of two reasons first of, osTicket constantlly converts urls and doing so is essential to its functioning. And secondly, the php files mentioned before that osTicket uses to run expect certain specific url and if those urls are not provided or are provided in a form that is not expected errors may arise</p>

<p>To enable url rewrite we have to run <code>rewrite_amd64_en-US</code></p>
<img src="https://i.imgur.com/Z2F3sie.png"  height="25%" width="25%"/>

<h3>Microsoft Visual C++ Redistributable</h3>
<p>when you creeate an aplication in the c progaming language family it is almost imposible that you dont rely on libraries</p>
<p>libraries can be think of as reusable code, like functions or classes that other people created and that you are reusing in your own aplication, using libraries is really common but it has a downside apps that are build using any specific library will then need that library to run even after the code is compiled into a <code>.exe</code> file</p>
<p>osTicket is affected by that due to the fact that some components it needs for its correct working really on those libraries</p>
<p>for instance we need to install said libraries to use:</p>
<code>certain php extentions</code>
<code>certain iis modules</code>
<code>image processing functions within osTicket</code>
<p>the specific libraries we need to install are the <code>Visual C++ Runtime libraries</code> and to install them we need to run <code>VC_redist.x86</code></p>

<h3>Deploying the webapp</h3>
<p>Now we have everything requiered for the osTicket webapp to run, but how do we actually run it?</p>
<p>Remember the <code>C:\inetpub\wwwroot</code> folder that i mentioned earlier</p>
<p>Well if we simply switch those files with the files that define ostciket we can convert our defoult webpage into osTicket</p>

<br>
<p>to find the necesarry files we can go to the <code>osTicket files zip</code> and extract its contents</p>
<p>once the extraction is finished we can just replace the files on the <code>C:\inetpub\wwwroot</code> folder with the files of the upload folder and our default webpage is now osTicket</p>
<img src="https://i.imgur.com/Rj8P2C3.png"  height="25%" width="25%"/>

<h3>Php Extentions</h3>

<h4>troubleshooting</h4>
some of these extensions may not appear so here are the links to download them
and we just go and add them
yeah likely problems with different versions
thay get copied

|Php extention name   |Php filename   |Download link   |
|---|---|---|
|PHP Programing language |https://downloads.php.net/~windows/releases/archives/php-8.5.5-Win32-vs17-x86.zip   |https://www.dllme.com/dll/files/php_intl/50f6c480829993069c42136ba74c6975 |
|PHP Manager for iis |https://github.com/RonaldCarter/PHPManager/releases/download/V1.5.0/PHPManagerForIIS_V1.5.0.msi   |https://downloads.php.net/~windows/pecl/releases/imap/1.0.3/php_imap-1.0.3-8.5-ts-vs17-x64.zip |
|URL Rewrite Installer   |https://download.microsoft.com/download/1/2/8/128E2E22-C1B9-44A4-BE2A-5859ED1D4592/rewrite_amd64_en-US.msi   |https://downloads.php.net/~windows/pecl/releases/imap/1.0.3/php_imap-1.0.3-8.4-ts-vs17-x64.zip |
|https://downloads.php.net/~windows/pecl/releases/imap/1.0.0/php_imap-1.0.0-8.3-ts-vs16-x64.zip   |https://download.visualstudio.microsoft.com/download/pr/40b59c73-1480-4caf-ab5b-4886f176bf71/435A0DE411B991E2BFC7FD1D5439639E7B32206960D3099370E36172018F52FE/VC_redist.x86.exe   |https://downloads.php.net/~windows/pecl/releases/imap/1.0.3/php_imap-1.0.3-8.5-ts-vs17-x64.zip  |
|osTicket installation files   |https://github.com/osTicket/osTicket/releases/download/v1.18.3/osTicket-v1.18.3.zip   |
