# pages
contact page
These are the home pages of any website

I've never liked to repeat the same job, this seems boring. That's why I created these pages and I spread the name of the site everywhere
to become dynamic.

- I certainly didn't post the name itself, it's a shortcode for the get_bloginfo("name") function.
- I ran file_get_contents() to get the content.

This saved me a lot of time. 😊

Let's play it on your site now:

in the function.php . file
Add the following code as it is:

```
<?php
function bloginfoDahab( $atts ) {
             extract(shortcode_atts(array(       'value' => '',   ), $atts));
             return get_bloginfo($value);
}
 
add_shortcode('bloginfo', 'bloginfoDahab'); //Replace the get_bloginfo function => this bloginfo shortcode

/*
* return the id of that page.
* https://bit.ly/how-to-auto-generat-pages
*/
function your_theme_create_page($page_title, $page_content)
{
$page_obj = get_page_by_title($page_title, 'OBJECT', 'page');
if ($page_obj) {
return false;
exit();
}
$page_args = array(
'post_type'      => 'page',
'post_status'    => 'publish',
'post_title'     => ucwords($page_title),
'post_name'      => strtolower(trim($page_title)),
'post_content'   => $page_content,
);
$page_id = wp_insert_post($page_args);
return $page_id;
}

# This time we will create an associative array to hold our page titles as well as page contents.
# Then we feed that array to the function we created above using a foreach loop.

add_action('after_switch_theme', 'my_custom_pages_on_theme_activation');

function my_custom_pages_on_theme_activation()
{

    $about_page_title = 'About Us';
	
    $cookies_policy_page_title = 'Cookies Policy'; 

    $privacy_policy_page_title   = 'Privacy Policy';
	
	$terms_of_service_page_title   = 'Terms of service';

    $dmca_page_title = 'DMCA';

    $about_page_content = file_get_contents('https://raw.githubusercontent.com/MuhammadDahab/pages/main/about.php');
	
    $cookies_policy_page_content   = file_get_contents('https://raw.githubusercontent.com/MuhammadDahab/pages/main/cookies_policy.php');
	
    $privacy_policy_page_content   = file_get_contents('https://raw.githubusercontent.com/MuhammadDahab/pages/main/privacy_policy.php');
	
	$terms_of_service_page_content   = file_get_contents('https://raw.githubusercontent.com/MuhammadDahab/pages/main/terms_of_service.php');

    $dmca_page_content = file_get_contents('https://raw.githubusercontent.com/MuhammadDahab/pages/main/dmca.php');

    $pages_array = array(

        $about_page_title => $about_page_content,
		
        $cookies_policy_page_title   => $cookies_policy_page_content,
		
        $privacy_policy_page_title   => $privacy_policy_page_content,
		
		$terms_of_service_page_title   => $terms_of_service_page_content,

        $dmca_page_title => $dmca_page_content

    );

    foreach ($pages_array as $page_title => $page_content) {

        $current_page = your_theme_create_page($page_title, $page_content);

        if (false != $current_page) {

            add_action('admin_notices', function () use ($page_title) {

?>
                <div class="notice notice-success is-dismissible">

                    <p><?php echo "Done! {$page_title} has been created"; ?></p>

                </div>

            <?php

            });

        } else {

            add_action('admin_notices', function () use ($page_title) {

            ?>

                <div class="notice notice-warning is-dismissible">

                    <p><?php echo "{$page_title} was not created! Check whether it already exists"; ?></p>

                </div>

<?php

            });

        }

    }

}
```

If you want to use a different file than the original file, you must call the functions using 👉 ```require_once```

That's all it is, oh, enjoy😋

 
