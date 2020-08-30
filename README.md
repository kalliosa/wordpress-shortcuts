# WordPress Shortcuts
My most used WordPress codes.

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
- [Loop Custom Posts](#loop-custom-posts)
- [Find ID of Top-Most Parent Page](#find-id)
- [Breadcrumbs](#breadcrumbs)

## Database
- [Replace old URL with new](#new-url)

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
### <a name="loop-custom-posts"></a>Loop Custom Posts
```
<?php
        $posts = new WP_Query(
        array(    
        'post_type' => 'mycpt',   
        'order' => 'DESC',
        'orderby' => 'date',
        'numberposts' => -1
          ) 
        ); ?>
          
          <?php while ( $posts->have_posts() ) : $posts->the_post(); ?>
            <h1><?php the_title(); ?></h1>
            <?php the_content(); ?>
          <?php endwhile; wp_reset_query(); ?>
```
### <a name="find-id"></a>Find ID of Top-Most Parent Page
```
<?php

if ($post->post_parent)	{
	$ancestors=get_post_ancestors($post->ID);
	$root=count($ancestors)-1;
	$parent = $ancestors[$root];
} else {
	$parent = $post->ID;
}

?>
```
### <a name="breadcrumbs"></a>Breadcrumbs
```
function haaja_breadcrumbs() {

	/* === OPTIONS === */
	$text['home']     = 'Etusivu'; // text for the 'Home' link
	$text['category'] = 'Arkisto "%s"'; // text for a category page
	$text['search']   = 'Hakutulokset hakusanalle "%s"'; // text for a search results page
	$text['tag']      = 'Kirjoitukset avainsanalla "%s"'; // text for a tag page
	$text['author']   = 'Kirjoittajan %s julkaisut'; // text for an author page
	$text['404']      = 'Virhe 404'; // text for the 404 page
	$text['page']     = 'Sivu %s'; // text 'Page N'
	$text['cpage']    = 'Kommenttisivu %s'; // text 'Comment Page N'

	$wrap_before    = '<div class="breadcrumbs" itemscope itemtype="http://schema.org/BreadcrumbList"><div class="container cf">'; // the opening wrapper tag
	$wrap_after     = '</div></div><!-- .breadcrumbs -->'; // the closing wrapper tag
	$sep            = '<span class="sep">&rsaquo;</span>'; // separator between crumbs
	$before         = '<span class="breadcrumbs__current">'; // tag before the current crumb
	$after          = '</span>'; // tag after the current crumb

	$show_on_home   = 1; // 1 - show breadcrumbs on the homepage, 0 - don't show
	$show_home_link = 1; // 1 - show the 'Home' link, 0 - don't show
	$show_current   = 1; // 1 - show current page title, 0 - don't show
	$show_last_sep  = 1; // 1 - show last separator, when current page title is not displayed, 0 - don't show
	/* === END OF OPTIONS === */

	global $post;
	$home_url       = home_url('/');
	$link           = '<span itemprop="itemListElement" itemscope itemtype="http://schema.org/ListItem">';
	$link          .= '<a class="breadcrumbs__link" href="%1$s" itemprop="item"><span itemprop="name">%2$s</span></a>';
	$link          .= '<meta itemprop="position" content="%3$s" />';
	$link          .= '</span>';
	$parent_id      = ( $post ) ? $post->post_parent : '';
	$home_link      = sprintf( $link, $home_url, $text['home'], 1 );

	if ( is_home() || is_front_page() ) {

		if ( $show_on_home ) echo $wrap_before . $home_link . '<span class="sep">&rsaquo;</span>' . __('Ajankohtaista', 'haaja') . $wrap_after;

	} else {

		$position = 0;

		echo $wrap_before;

		if ( $show_home_link ) {
			$position += 1;
			echo $home_link;
		}

		if ( is_category() ) {
			$parents = get_ancestors( get_query_var('cat'), 'category' );
			foreach ( array_reverse( $parents ) as $cat ) {
				$position += 1;
				if ( $position > 1 ) echo $sep;
				echo sprintf( $link, get_category_link( $cat ), get_cat_name( $cat ), $position );
			}
			if ( get_query_var( 'paged' ) ) {
				$position += 1;
				$cat = get_query_var('cat');
				echo $sep . sprintf( $link, get_category_link( $cat ), get_cat_name( $cat ), $position );
				echo $sep . $before . sprintf( $text['page'], get_query_var( 'paged' ) ) . $after;
			} else {
				if ( $show_current ) {
					if ( $position >= 1 ) echo $sep;
					echo $before . sprintf( $text['category'], single_cat_title( '', false ) ) . $after;
				} elseif ( $show_last_sep ) echo $sep;
			}

		} elseif ( is_search() ) {
			if ( get_query_var( 'paged' ) ) {
				$position += 1;
				if ( $show_home_link ) echo $sep;
				echo sprintf( $link, $home_url . '?s=' . get_search_query(), sprintf( $text['search'], get_search_query() ), $position );
				echo $sep . $before . sprintf( $text['page'], get_query_var( 'paged' ) ) . $after;
			} else {
				if ( $show_current ) {
					if ( $position >= 1 ) echo $sep;
					echo $before . sprintf( $text['search'], get_search_query() ) . $after;
				} elseif ( $show_last_sep ) echo $sep;
			}

		} elseif ( is_year() ) {
			if ( $show_home_link && $show_current ) echo $sep;
			if ( $show_current ) echo $before . get_the_time('Y') . $after;
			elseif ( $show_home_link && $show_last_sep ) echo $sep;

		} elseif ( is_month() ) {
			if ( $show_home_link ) echo $sep;
			$position += 1;
			echo sprintf( $link, get_year_link( get_the_time('Y') ), get_the_time('Y'), $position );
			if ( $show_current ) echo $sep . $before . get_the_time('F') . $after;
			elseif ( $show_last_sep ) echo $sep;

		} elseif ( is_day() ) {
			if ( $show_home_link ) echo $sep;
			$position += 1;
			echo sprintf( $link, get_year_link( get_the_time('Y') ), get_the_time('Y'), $position ) . $sep;
			$position += 1;
			echo sprintf( $link, get_month_link( get_the_time('Y'), get_the_time('m') ), get_the_time('F'), $position );
			if ( $show_current ) echo $sep . $before . get_the_time('d') . $after;
			elseif ( $show_last_sep ) echo $sep;

		} elseif ( is_single() && ! is_attachment() ) {
			if ( get_post_type() != 'post' ) {
				$position += 1;
				$post_type = get_post_type_object( get_post_type() );
				if ( $position > 1 ) echo $sep;
				echo sprintf( $link, get_post_type_archive_link( $post_type->name ), $post_type->labels->name, $position );
				if ( $show_current ) echo $sep . $before . get_the_title() . $after;
				elseif ( $show_last_sep ) echo $sep;
			} else {
				$cat = get_the_category(); $catID = $cat[0]->cat_ID;
				$parents = get_ancestors( $catID, 'category' );
				$parents = array_reverse( $parents );
				$parents[] = $catID;
				foreach ( $parents as $cat ) {
					$position += 1;
					if ( $position > 1 ) echo $sep;
					echo sprintf( $link, get_category_link( $cat ), get_cat_name( $cat ), $position );
				}
				if ( get_query_var( 'cpage' ) ) {
					$position += 1;
					echo $sep . sprintf( $link, get_permalink(), get_the_title(), $position );
					echo $sep . $before . sprintf( $text['cpage'], get_query_var( 'cpage' ) ) . $after;
				} else {
					if ( $show_current ) echo $sep . $before . get_the_title() . $after;
					elseif ( $show_last_sep ) echo $sep;
				}
			}

		} elseif ( is_post_type_archive() ) {
			$post_type = get_post_type_object( get_post_type() );
			if ( get_query_var( 'paged' ) ) {
				$position += 1;
				if ( $position > 1 ) echo $sep;
				echo sprintf( $link, get_post_type_archive_link( $post_type->name ), $post_type->label, $position );
				echo $sep . $before . sprintf( $text['page'], get_query_var( 'paged' ) ) . $after;
			} else {
				if ( $show_home_link && $show_current ) echo $sep;
				if ( $show_current ) echo $before . $post_type->label . $after;
				elseif ( $show_home_link && $show_last_sep ) echo $sep;
			}

		} elseif ( is_attachment() ) {
			$parent = get_post( $parent_id );
			$cat = get_the_category( $parent->ID ); $catID = $cat[0]->cat_ID;
			$parents = get_ancestors( $catID, 'category' );
			$parents = array_reverse( $parents );
			$parents[] = $catID;
			foreach ( $parents as $cat ) {
				$position += 1;
				if ( $position > 1 ) echo $sep;
				echo sprintf( $link, get_category_link( $cat ), get_cat_name( $cat ), $position );
			}
			$position += 1;
			echo $sep . sprintf( $link, get_permalink( $parent ), $parent->post_title, $position );
			if ( $show_current ) echo $sep . $before . get_the_title() . $after;
			elseif ( $show_last_sep ) echo $sep;

		} elseif ( is_page() && ! $parent_id ) {
			if ( $show_home_link && $show_current ) echo $sep;
			if ( $show_current ) echo $before . get_the_title() . $after;
			elseif ( $show_home_link && $show_last_sep ) echo $sep;

		} elseif ( is_page() && $parent_id ) {
			$parents = get_post_ancestors( get_the_ID() );
			foreach ( array_reverse( $parents ) as $pageID ) {
				$position += 1;
				if ( $position > 1 ) echo $sep;
				echo sprintf( $link, get_page_link( $pageID ), get_the_title( $pageID ), $position );
			}
			if ( $show_current ) echo $sep . $before . get_the_title() . $after;
			elseif ( $show_last_sep ) echo $sep;

		} elseif ( is_tag() ) {
			if ( get_query_var( 'paged' ) ) {
				$position += 1;
				$tagID = get_query_var( 'tag_id' );
				echo $sep . sprintf( $link, get_tag_link( $tagID ), single_tag_title( '', false ), $position );
				echo $sep . $before . sprintf( $text['page'], get_query_var( 'paged' ) ) . $after;
			} else {
				if ( $show_home_link && $show_current ) echo $sep;
				if ( $show_current ) echo $before . sprintf( $text['tag'], single_tag_title( '', false ) ) . $after;
				elseif ( $show_home_link && $show_last_sep ) echo $sep;
			}

		} elseif ( is_author() ) {
			$author = get_userdata( get_query_var( 'author' ) );
			if ( get_query_var( 'paged' ) ) {
				$position += 1;
				echo $sep . sprintf( $link, get_author_posts_url( $author->ID ), sprintf( $text['author'], $author->display_name ), $position );
				echo $sep . $before . sprintf( $text['page'], get_query_var( 'paged' ) ) . $after;
			} else {
				if ( $show_home_link && $show_current ) echo $sep;
				if ( $show_current ) echo $before . sprintf( $text['author'], $author->display_name ) . $after;
				elseif ( $show_home_link && $show_last_sep ) echo $sep;
			}

		} elseif ( is_404() ) {
			if ( $show_home_link && $show_current ) echo $sep;
			if ( $show_current ) echo $before . $text['404'] . $after;
			elseif ( $show_last_sep ) echo $sep;

		} elseif ( has_post_format() && ! is_singular() ) {
			if ( $show_home_link && $show_current ) echo $sep;
			echo get_post_format_string( get_post_format() );
		}

		echo $wrap_after;

	}
}
```
## Database
### <a name="new-url"></a>Replace old URL with new
```
update wp_posts set post_content = replace(post_content, 'http:\/\/oldurl.com', 'http:\/\/newurl.com');
```
