# Wordpress Shortcuts
My most used WordPress shortcuts

- [Add image size](#add-image-size)
- [Set Theme root](#themeroot)
- [Set Images folder](#images-folder)

## <a name="themeroot"></a>Theme root
```
define('THEMEROOT', get_stylesheet_directory_uri());
```
## <a name="images-folder"></a> Set Images folder
```
define('IMAGES', THEMEROOT . '/images');
```
## <a name="add-image-size"></a>Add image size
```
add_image_size( 'imagename', 130, 180, true );
```
