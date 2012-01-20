<h2 id="launching-your-drupal-app">Launching Your Drupal App</h2>
<h4 id="-create-a-local-instance-of-your-app">Create a Local Instance of Drupal</h4>
<p>We&rsquo;re assuming that you&rsquo;ve already done this. If you haven&rsquo;t and don&rsquo;t know how, here&rsquo;s a couple links for you:<br />
<a href="http://drupal.org/node/66187" target="_blank">Create a local Drupal instance on a Mac</a><br />
a href="http://drupal.org/documentation/install/windows" target="_blank">Create a local Drupal instance in Windows</a></p>
<h3 class="tag"><span class="guides-sprite w-cap">&nbsp;</span><span class="horizontal-guides-sprite white">Important: No Auto-Installers on Pagoda Box</span><span class="guides-sprite w-end-cap">&nbsp;</span></h3>
<p>You are not able to run auto-installers on Pagoda Box. If you use Drupal&#39;s auto-installer, run it locally to generate all the necessary configuration files, then deploy your app to Pagoda Box.</p>
<h4 id="display-errors">Display Errors</h4>
<p>During development, we recommend configuring Drupal to display errors in the admin panel. By default Drupal is configured to display &quot;All messages&quot;, so unless you&#39;ve changed that, you should be ready to go.</p>
<h4 id="-create-a-github-repo-for-your-app">Create a Git Repo for Your Drupal App</h4>
<p><a href="/customer/portal/articles/202225-setting-up-git">Set up Git</a>, if you haven't already. Then initialize your app as a git repo.</p>	
<h4 id="drupal-into-subdir">Move Drupal into Subdirectory</h4>
<p>Because Drupal needs a Temporary Directory that is not accessible over the web, move your entire Drupal application(excluding .git/ & .gitignore) into a subdirectory. We suggest naming the directory "drupal" so it coincides with the below Boxfile. You don't need to create the actual temporary directory as the below Boxfile will create it for you when your app is deployed.</p>
	<div class="vertical-guides-sprite image">
	  <span class="guides-sprite top"></span>
	  <img src="http://assistly-assets.pagodabox.com/images/guides/drupal-sub-dir-sml.png" alt="" />
	  <a href="#" targ="http://assistly-assets.pagodabox.com/images/guides/drupal-sub-dir.png" class="guides-sprite zoom"></a>
	  <span class="guides-sprite bottom">&nbsp;</span>
	</div>
	<h4 id="-edit-gitignore-file">Edit .gitignore file</h4>
	<p>Depending on your version of Drupal, it may have come with a default .gitignore file. Remove the line with the settings.php file as Pagoda will need this included in your repo. Then also prepend the "files" and "private" directories with the new "drupal" directory added in the last step.</p>
	<h3 id="gitignore" class="tag"><span class="guides-sprite cap">&nbsp;</span><span class="horizontal-guides-sprite title">GIT</span><span class="horizontal-guides-sprite green">Edit .gitignore File</span><span class="guides-sprite green-end-cap"></span></h3>
	<div class="block grey code">
	 <script class='brush: plain; class-name: strike' type='syntaxhighlighter'>
	   <![CDATA[
       sites/*/settings*.php
       sites/*/files
       sites/*/private
	   ]]>
	 </script>
	 <script class='brush: plain;' type='syntaxhighlighter'>
	   <![CDATA[
       drupal/sites/*/files
       drupal/sites/*/private
	   ]]>
	 </script>
	 <div class="extra">
	   /.gitignore
	 </div>
	</div>
	<h4 id="-the-all--powerful-box-file">The All-Powerful Boxfile</h4>
  <p>Create a file named &quot;Boxfile&quot; in the base directory of your git repo and paste in the following code snippet. The Boxfile is your Pagoda Box config file. If you want the Boxfile explained in more detail, click <a href="/customer/portal/articles/175475">here</a>.</p>
	<h3 class="tag" id="drupal-default-box-file"><span class="guides-sprite cap">&nbsp;</span><span class="horizontal-guides-sprite title">PHP</span><span class="horizontal-guides-sprite green">Drupal Boxfile Example</span><span class="guides-sprite green-end-cap">&nbsp;</span></h3>
	<div class="block grey code" id="default-box-config-settings">
    <script class='brush: php' type='syntaxhighlighter'>
      <![CDATA[
          web1: #component type & number
            name: drupal #component settings
            document_root: /drupal
            shared_writable_dirs:
              - drupal/sites/default/files
              - tmp
            php_extensions:
              - mysqli
              - gd
              - hash
              - json
              - xml
              - pdo
              - pdo_mysql
              - mcrypt
              - mbstring
              - eaccelerator #recommended, but optional
            db1: #component type & number
              name: drupal-db #component settings
              type: mysql
      ]]>
    </script>
    <div class="extra">
	    /Boxfile
	  </div>
	</div>
	<p>Identifying sites/default/files as a writable directory will allow your application to write images, uploads, etc. to that directory and then share them among application clones. The PHP extensions are those required by the <a href="http://drupal.org/requirements" target="_blank">Drupal System Requirements</a>.</p>
	<p>Your directory structure should now look something like this:</p>
	<div class="vertical-guides-sprite image">
	  <span class="guides-sprite top"></span>
	  <img src="http://assistly-assets.pagodabox.com/images/guides/drupal-dir-sml.png" alt="" />
	  <a href="#" targ="http://assistly-assets.pagodabox.com/images/guides/drupal-dir.png" class="guides-sprite zoom"></a>
	  <span class="guides-sprite bottom">&nbsp;</span>
	</div>
	<p>If you want to know more about writing your own or customizing your Boxfile, check out <a href="/customer/portal/articles/175475">Boxfile Guide</a>.</p>
	<h4 id="local_vs._live_environments">Local vs. Live Environments</h4>
	<p>Since Drupal doesn&rsquo;t natively accommodate database settings for multiple environments(Local vs Live), we recommend adding some additional logic to your sites/default/settings.php file to dynamically switch database settings based on an environment variable. Note that the Pagoda database credentials have been replaced with auto-created <a href="/customer/portal/articles/175470">global variables</a> instead of the actual credentials. But, you can also hard code the <a href="/customer/portal/articles/175426-creating-a-database#-connecting-to-your-db">credentials</a> if you prefer.</p>
	<h3 class="tag">
		<span class="guides-sprite cap">&nbsp;</span><span class="horizontal-guides-sprite title">PHP</span><span class="horizontal-guides-sprite green">Set Your Database Credentials</span><span class="guides-sprite green-end-cap">&nbsp;</span></h3>
	<div class="block grey code" id="config-for-your-new-database">
    <script class='brush: php; class-name: strike' type='syntaxhighlighter'>
      <![CDATA[
          $databases = array (
            'default' => 
            array (
              'default' => 
              array (
                'database' => 'local_db_name',
                'username' => 'local_db_user',
                'password' => 'local_db_pass',
                'host' => 'local_db_host',
                'port' => '',
                'driver' => 'mysql',
                'prefix' => '',
              ),
            ),
          );
      ]]>
		</script>
		<script class='brush: php;' type='syntaxhighlighter'>
	    <![CDATA[
          if (isset($_SERVER['PLATFORM']) && $_SERVER['PLATFORM'] == 'PAGODABOX') {
              $databases = array (
                'default' => 
                array (
                  'default' => 
                  array (
                    'database' => $_SERVER['DB1_NAME'],
                    'username' => $_SERVER['DB1_USER'],
                    'password' => $_SERVER['DB1_PASS'],
                    'host' => $_SERVER['DB1_HOST'],
                    'port' => $_SERVER['DB1_PORT'],
                    'driver' => 'mysql',
                    'prefix' => '',
                  ),
                ),
              );
          }

          else {
              $databases = array (
                'default' => 
                array (
                  'default' => 
                  array (
                    'database' => 'drupal-demo',
                    'username' => 'root',
                    'password' => 'root',
                    'host' => 'localhost',
                    'port' => '',
                    f'driver' => 'mysql',
                    'prefix' => '',
                  ),
                ),
              );
          }
      ]]>
		</script>
		<div class="extra">
			/sites/default/settings.php
		</div>
	</div>
	<h4 id="-deploy-your-app-on-pagoda-box">Deploy Your App on Pagoda Box</h4>
	<p>Just go through the normal process of <a href="/customer/portal/articles/174146-launching-your-first-app">deploying your app</a>.</p>
	<h4 id="create-database">Create a Database</h4>
	<p>If you used the example Boxfile from above, an empty database has already been created for you. If not, you can manually <a href="/customer/portal/articles/175426-creating-a-database#creating-db-through-dashboard">create a database from the dashboard</a>.</p>
	<h4 id="-environment-variable">Create Environment Variable</h4>
	<p>Create an <a href="/customer/portal/articles/175470">environment variable</a> with the key and value of PLATFORM = PAGODABOX so your app will know which database to connect to.</p>
	<h4 id="get-your-local-database-to-pagoda">Get your local database to Pagoda</h4>
	<p>Export your local database with your favorite local database administration tool(e.g. phpMyAdmin, Sequel Pro, etc.).</p>
	<p>Import your database into your live app on Pagoda using the <a href="/customer/portal/articles/175427">database tunnel</a></p>
	<p>Now when you go to yourapp.pagodabox.com you should see your Drupal app running on Pagoda!</p>
	<p>Lastly, configure Drupal to use the "tmp" you created in your Boxfile, by going to yourapp.pagodabox.com/admin/config/media/file-system and entering "/tmp" as the "Temporary directory".</p>
	<h3 class="tag">
		<span class="guides-sprite w-cap">&nbsp;</span><span class="horizontal-guides-sprite white">Quick Note</span><span class="guides-sprite w-end-cap">&nbsp;</span></h3>
	<div class="block yellow">
		<p>If you set <a href="#display-errors">errors to display</a>, now would be a good time to turn it off, unless your still debugging your Drupal app.</p>
		<p>There&rsquo;s a few more things you can do to make your app fully functional that are covered in these other guides:<br />
			- <a href="/customer/portal/articles/175384">Sending Mail from Your App</a><br />
			- <a href="/customer/portal/articles/175471">Creating a DNS Alias</a><br />
			- <a href="/customer/portal/articles/175459">Scaling your App</a>(Coming)</p>
<h2 id="post-launch-workflow-recommendations">Post-launch Workflow Recommendations</h2>
<div class="justify">
	<h4 id="-things-to-note">Things to Note</h4>
	<p>Pagoda Box makes managing and updating your Drupal app really easy, but there&rsquo;s some things you should know.</p>
	<ol>
		<li>
			<p>For security reasons, Pagoda Box only allows SSH or FTP access to your <a href="/customer/portal/articles/175418">writable directories</a>, but not your application code. This means that any updates, plugins, themes, or any other code changes need to be done on your local instance first, then pushed to your repo and deployed through your app dashboard.</p>
		</li>
		<li>
			<p>Remember that your local database and your live database are two completely separate databases. Anytime you want to publish a new post or add media to your Drupal app, it needs to be done through your live admin, not your local admin. Your Drupal content is not stored in your repo so any content you add locally will not be pushed live with your repo.</p>
		</li>
	</ol>
</div>