Created by Kaspars Dambis [konstruktors.com](http://konstruktors.com/)

## Important:

* 	This plugin is intended for a local development environment.
* 	It is much easier to use Apache's virtual hosts setup and modify your hosts file locally instead of using this plugin.
* 	This is a MU plugin, so you should put it in `/wp-content/mu-plugins/multisite-local-dev.php`
* 	It works only with Networks where sites are in sub-folders. However, it should be easy to modify this script to support sites in subdomains.
* 	Blog post: http://konstruktors.com/blog/wordpress/3857-develop-wordpress-multisite-locally-using-production-database/


### Network Setup (Production):

*	**Network home URL (blog_id 1):** `http://example.com/`
*	**Singe site URLs (blog_id 2):** `http://example.com/extranet/`
*	**Singe site URLs (blog_id 3):** `http://example.com/intranet/`

### Network Setup (Development):

*	**Network home URL (blog_id 1):** `http://localhost/example-dev/public/`
*	**Singe site URLs (blog_id 2):** `http://localhost/example-dev/public/extranet/`
*	**Singe site URLs (blog_id 3):** `http://localhost/example-dev/public/intranet/`


You need to put this into your dev `wp-config.php` right below the Multisite related constants:

	// (int) blog_id => (string) blog_path

	$dev_sites = array(
		1 => '', // Network home
		2 => 'extranet',
		3 => 'intranet'
	);

	define('WP_CONTENT_URL', 'http://localhost/example-dev/public/wp-content');

	$current_site = new stdClass;
	$current_site->id = 1;
	$current_site->site_id = 1;
	$current_site->domain = DOMAIN_CURRENT_SITE;
	$current_site->path = PATH_CURRENT_SITE;
	$current_site->cookie_domain = DOMAIN_CURRENT_SITE;

	foreach ( $dev_sites as $site_id => $folder )
		if ( strstr( $_SERVER['REQUEST_URI'], '/' . $folder . '/' ) )
			$current_site->blog_id = $site_id;

	if ( ! $current_site->blog_id )
		$current_site->blog_id = 1;

	$current_blog = $current_site;

