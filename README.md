# WordPress Shortcuts
My most used WordPress shortcuts

## WordPress Functions
- [Set Theme root](#themeroot)
- [Set Images folder](#images-folder)
- [Load jQuery and other Scripts after footer](#load-scripts)
- [Register Navigation menus](#register-navs)
- [Register Sidebars](#sidebars)
- [Thumbnails and Image Sizes](#add-image-size)
- [Pagination (by Kriesi)](#pagination-kriesi)
- [Custom Post Types](#custom-post-types)
- [Custom Post Taxonomies](#custom-taxonomies)

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
    'name' => 'Main Menu',
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
### <a name="pagination-kriesi"></a>Pagination (by Kriesi)
From [Kriesi's blog](http://www.kriesi.at/archives/how-to-build-a-wordpress-post-pagination-without-plugin)
```
function kriesi_pagination($pages = '', $range = 2)
{  
     $showitems = ($range * 2)+1;  

     global $paged;
     if(empty($paged)) $paged = 1;

     if($pages == '')
     {
         global $wp_query;
         $pages = $wp_query->max_num_pages;
         if(!$pages)
         {
             $pages = 1;
         }
     }   

     if(1 != $pages)
     {
         echo "<div class='pagination'>";
         if($paged > 2 && $paged > $range+1 && $showitems < $pages) echo "<a href='".get_pagenum_link(1)."'>&laquo;</a>";
         if($paged > 1 && $showitems < $pages) echo "<a href='".get_pagenum_link($paged - 1)."'>&lsaquo;</a>";

         for ($i=1; $i <= $pages; $i++)
         {
             if (1 != $pages &&( !($i >= $paged+$range+1 || $i <= $paged-$range-1) || $pages <= $showitems ))
             {
                 echo ($paged == $i)? "<span class='current'>".$i."</span>":"<a href='".get_pagenum_link($i)."' class='inactive' >".$i."</a>";
             }
         }

         if ($paged < $pages && $showitems < $pages) echo "<a href='".get_pagenum_link($paged + 1)."'>&rsaquo;</a>";  
         if ($paged < $pages-1 &&  $paged+$range-1 < $pages && $showitems < $pages) echo "<a href='".get_pagenum_link($pages)."'>&raquo;</a>";
         echo "</div>\n";
     }
}
```
Code for template:
```
kriesi_pagination();
```
### <a name="custom-post-types"></a>Custom Post Types
Register post type
```
add_action( 'init', 'create_mycpt' );

function create_mycpt() {
    register_post_type( 'postname',
        array(
            'labels' => array(
                'name' => 'MYCPT',
                'singular_name' => 'Mycpt',
                'add_new' => 'Add new',
                'add_new_item' => 'Add new custom post ',
                'edit' => 'Edit',
                'edit_item' => 'Edit custom post',
                'new_item' => 'New custom post',
                'view' => 'View',
                'view_item' => 'View post',
                'search_items' => 'Search posts',
                'not_found' => 'No custom posts',
                'not_found_in_trash' => 'Trash is empty',
                'parent' => ''
            ),
            'public' => true,
            'menu_position' => 15,
            'menu_icon' => IMAGES . '/box-icon.png',
            'supports' => array( 'title', 'thumbnail', 'editor' ),
            'taxonomies' => array( '' ),
            'has_archive' => false,
            'publicly_queryable' => false
        )
    );
}
```
### <a name="custom-taxonomies"></a>Custom Post Taxonomies
Add Categories to Custom Post Types
```
add_action( 'init', 'create_my_taxonomies', 0 );

function create_my_taxonomies() {
    register_taxonomy(
        'mycpt_cat',
        'mycpt',
        array(
            'labels' => array(
                'name' => 'Category name',
                'add_new_item' => 'Add new',
                'new_item_name' => "Add new"
            ),
            'show_ui' => true,
            'show_tagcloud' => false,
            'hierarchical' => true
        )
    );
}
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
