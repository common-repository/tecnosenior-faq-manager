=== Tecnosenior FAQ Manager ===
Contributors: pedrokiefer
Tags: FAQ, FAQ Manager, ask questions, search questions, ajax, ajax admin interfaces, shortcode, custom shortcode skins, fulltext search, faq search, mvc based plugin
Requires at least: 3.3
Tested up to: 3.4.1
Stable tag: trunk
License: GPLv2, BSD, LGPLv2

A Wordpress FAQ Manager where you can easily manage questions on your site.

== Description ==

*Features*:

* MVC Based Plugin
* AJAX Based Administration Interfaces
* Skins for shortcode tag: you can create your own skin!
* Full text search, using mysql FULLTEXT support
* Email notifications
* Easily Translatable
* I18N: English and Brazilian Portuguese

Development happens on [github](https://github.com/pedrokiefer/tsc-faq-manager)

== Installation ==

1. Upload zip file to `wp-content/plugins` directory and extract it
2. Activate the plugin through the 'Plugins' menu in WordPress
3. Place the shortcode `[tscfaq id=?]` substituting ? for a group id
4. Customize the shortcode skin

== Frequently Asked Questions ==

None for the moment.

== Screenshots ==

None.

== Changelog ==

= 1.2 =
First release to wordpress.org

== Upgrade Notice ==

None.

== Customizations ==

## Skins
To create a skin, just add a new directory under skins with your new skin. Inside the directory there should be a PHP
file with a custom header and two functions. Those functions are called by the shortcode renderer, don't change the name of
the functions!

### Skin Header
The file header is composed of the following fields, just like any Wordpress theme or plugin - inside a multi line comment on 
the first 8kb of the file.

* Skin Name
* Description
* Version
* Author
* Author URI

### Functions
The following functions should be on your skin file:

	function tsc_skin_get_headers()
	function tsc_skin_render($group, $questions)

The first function is responsible for adding to the headers any needed javascript or css. It must return an array with the keys `js` and `css`. The code below include jquery, jquery-ui and a custom javascript file. It also include the needed css file.

	function tsc_skin_get_headers()
	{
    	$headers = array(
        	"js" => array(
            	"jquery",
            	"jquery-ui",
            	array("name" => "tecnosenior-faq", "file" => plugin_dir_url(__FILE__) . "/tecnosenior-faq.js")
        		),
        	"css" => array("tecnosenior-faq" => plugin_dir_url(__FILE__) . "/tecnosenior-faq.css")
    		);
	
    	return $headers;
	}	

The second function receive two objects containing the data that should be rendered. It must return a string. 

### Group
A single group object with this fields:
- Id
- GroupName
- SearchBox
- AskBox
- Status
- CreationDate

### Questions
An array of question objects with this fields available:
- Id
- GroupId
- QuestionOrder
- Question
- WhoAsked
- Answer
- Status
- Type
- CreationDate

Sample code, for skin renderer: 

	function tsc_skin_render($group, $questions)
	{
    	$html = "";
    	if ($group->SearchBox) {
        	$html .= render_search_box($group->Id);
    	}
	
    	$i = 0;
    	$html .= "<div class=\"span-14 prepend-1 append-1 last faq-questions\" id=\"faq-questions-list\">\n";
    	foreach ($questions as $q) {
        	$html .= render_question($q, ($i % 2 == 1));
        	$i++;
    	}
    	$html .= "</div>";
	
    	if ($group->AskBox) {
        	$html .= render_ask_box($group->Id);
    	}
	
    	return $html;
	}
