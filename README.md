## **Laravel WP Fetch Post with Image**

### This PDF explains the process of fetching posts with featured images from a WordPress API using

### Laravel.

### 1\. The BlogController:

###  \- Fetches WordPress posts with the \`\_embed\` parameter to include featured images.

###  \- Loops through the posts to check if a featured image exists.

###  \- If the featured image is found, it is added to the post's data.

###  \- If the featured image does not exist, a default image is used.

### Code example:

### **Updated Blade Template (`blog.blade.php`):**

| \<?phpnamespace App\\Http\\Controllers;use Illuminate\\Http\\Request;class BlogController extends Controller{    public function index()    {        // Fetch WordPress posts with embedded media        $json \= file\_get\_contents('https://example.com/wp-json/wp/v2/posts?\_embed');        $posts \= json\_decode($json, true);        // Loop through posts to extract featured images        foreach ($posts as &$post) {            // Check if the post has a featured image in the '\_embedded' field            if (isset($post\['\_embedded'\]\['wp:featuredmedia'\]\[0\]\['source\_url'\])) {                $post\['featured\_image'\] \= $post\['\_embedded'\]\['wp:featuredmedia'\]\[0\]\['source\_url'\];            } else {                // Default image if no featured image is found                $post\['featured\_image'\] \= asset('images/default-image.jpg');            }        }        // Return the view with posts and images        return view('blog', compact('posts'));    }} |
| :---- |

### **Explanation:**

* The code fetches posts from the WordPress API with `_embed` to include featured images.  
* It loops through each post to check if the featured image exists (`_embedded['wp:featuredmedia'][0]['source_url']`).  
* If the featured image is not found, it falls back to a default image (`default-image.jpg`).

### **Blade Template (`blog.blade.php`):**

In your Blade template, display the featured image like this:

| @foreach ($posts as $post)    \<div class="post"\>        \<h2\>{{ $post\['title'\]\['rendered'\] }}\</h2\>        \<img src="{{ $post\['featured\_image'\] }}" alt="Featured Image"\>        \<p\>{\!\! $post\['excerpt'\]\['rendered'\] \!\!}\</p\> \<\!-- Limiting the excerpt text to avoid overflow and showing a summary \--\>         \<p\>{\!\! Str::limit(strip\_tags($post\['excerpt'\]\['rendered'\]), 130) \!\!}\</p\>        \<a href="{{ $post\['link'\] }}"\>Read More\</a\>    \</div\>@endforeach |
| :---- |

Mamun Miah  
**Website:** mdmamunmiah.com  
**Email:** mamun.miah.dev@gmail.com
