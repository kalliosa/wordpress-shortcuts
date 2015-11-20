# Wordpress Shortcuts
My most used WordPress shortcuts

## WordPress Functions
- [Set Theme root](#themeroot)
- [Set Images folder](#images-folder)
- [Load jQuery and other Scripts after footer](#load-scripts)
- [Register Navigation menus](#register-navs)
- [Register Sidebars](#sidebars)
- [Thumbnails and Image Sizes](#add-image-size)

## Plugins
- [Attachments Plugin](#attachments)

## WordPress Functions
### <a name="themeroot"></a>Theme root
```
define('THEMEROOT', get_stylesheet_directory_uri());
```
### <a name="images-folder"></a> Set Images folder
```
define('IMAGES', THEMEROOT . '/images');
```
### <a name="load-scripts"></a>Load jQuery and other Scripts after footer
```
function my_scripts_method() {
    wp_deregister_script( 'jquery' );
    wp_register_script( 'jquery', 'https://ajax.googleapis.com/ajax/libs/jquery/1.11.3/jquery.min.js', array(), false, true);
    wp_enqueue_script( 'jquery' );
}    
add_action('wp_enqueue_scripts', 'my_scripts_method');
```
### <a name="register-navs"></a>Register Navigation menus
```
register_nav_menus( array(
    'name' => 'Admin Side Name',
    'name-2' => 'Second Menu'
) );
```
### <a name="sidebars"></a>Sidebars
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
### <a name="add-image-size"></a>Thumbnais and Images Sizes
#### Set Default Thumbnail Size
```
set_post_thumbnail_size( 50, 50, false ); // false = images will be scaled, true = images will be cropped
```
#### Add new Image Size
```
add_image_size( 'imagename', 130, 180, true ); // false = images will be scaled, true = images will be cropped
```

## Plugins
### <a name="attachments"></a>Attachments Plugin
[Plugin site](https://wordpress.org/plugins/attachments/)

#### Default Instance False
```
define( 'ATTACHMENTS_DEFAULT_INSTANCE', false );
```
#### functions.php Attachments Settings
```
function my_attachments( $attachments )
{
  $args = array(
    // title of the meta box (string)
    'label'         => 'My Attachments',
    // all post types to utilize (string|array)
    'post_type'     => array( 'page' ),
    // allowed file type(s) (array) (image|video|text|audio|application)
    'filetype'      => null,  // no filetype limit
    // include a note within the meta box (string)
    'note'          => 'Attach files here!',
    // text for 'Attach' button in meta box (string)
    'button_text'   => __( 'Attach Files', 'attachments' ),
    // text for modal 'Attach' button (string)
    'modal_text'    => __( 'Attach', 'attachments' ),
    /**
     * Fields for the instance are stored in an array. Each field consists of
     * an array with three keys: name, type, label.
     *
     * name  - (string) The field name used. No special characters.
     * type  - (string) The registered field type.
     *                  Fields available: text, textarea
     * label - (string) The label displayed for the field.
     */
    'fields'        => array(
      array(
        'name'  => 'title',                          // unique field name
        'type'  => 'text',                           // registered field type
        'label' => __( 'Title', 'attachments' ),     // label to display
      ),
      array(
        'name'  => 'caption',                        // unique field name
        'type'  => 'textarea',                       // registered field type
        'label' => __( 'Caption', 'attachments' ),   // label to display
      ),
      array(
        'name'  => 'copyright',                      // unique field name
        'type'  => 'text',                           // registered field type
        'label' => __( 'Copyright', 'attachments' ), // label to display
      ),
    ),
  );
  $attachments->register( 'my_attachments', $args ); // unique instance name
}
add_action( 'attachments_register', 'my_attachments' );
```
