# Wordpress Shortcuts
My most used WordPress shortcuts

- [Set Theme root](#themeroot)
- [Set Images folder](#images-folder)
- [Load jQuery and other Scripts after footer](#load-scripts)
- [Register Navigation menus](#register-navs)
- [Register Sidebars](#sidebars)
- [Thumbnails and Image Sizes](#add-image-size)

## <a name="themeroot"></a>Theme root
```
define('THEMEROOT', get_stylesheet_directory_uri());
```
## <a name="images-folder"></a> Set Images folder
```
define('IMAGES', THEMEROOT . '/images');
```
## <a name="load-scripts"></a>Load jQuery and other Scripts after footer
```
function my_scripts_method() {
    wp_deregister_script( 'jquery' );
    wp_register_script( 'jquery', 'https://ajax.googleapis.com/ajax/libs/jquery/1.11.3/jquery.min.js', array(), false, true);
    wp_enqueue_script( 'jquery' );
}    
add_action('wp_enqueue_scripts', 'my_scripts_method');
```
## <a name="register-navs"></a>Register Navigation menus
```
register_nav_menus( array(
    'name' => 'Admin Side Name',
    'name-2' => 'Second Menu'
) );
```
## <a name="sidebars"></a>Sidebars
```
register_sidebar( array(
		'name' => __( 'Main Sidebar', 'sivut' ),
		'id' => 'sidebar-1',
		'before_widget' => '<aside id="%1$s" class="widget %2$s">',
		'after_widget' => "</aside>",
		'before_title' => '<h3 class="widget-title">',
		'after_title' => '</h3>',
	) );
```
## <a name="add-image-size"></a>Thumbnais and Images Sizes
### Set Default Thumbnail Size (scaled)
```
set_post_thumbnail_size( 50, 50, false ); // false = images will be scaled, true = images will be cropped
```
### Add new Image Size
```
add_image_size( 'imagename', 130, 180, true ); // false = images will be scaled, true = images will be cropped
```
