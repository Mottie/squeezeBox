SqueezeBox adds a whole page sliding accordion effect that was originally seen on the [Apple Store site][1] (Click on Compare a Mac, then Compare). This plugin emulates the same effect with some improvements. [Check out the demo][2]!

**Usage**

**Default setup**

* **Page Head**

        <!-- jQuery UI Redmond theme -->
        <link rel="stylesheet" href="http://ajax.googleapis.com/ajax/libs/jqueryui/1.8/themes/redmond/jquery-ui.css" type="text/css" />
        <!-- Use the CSS below while using a jQuery UI theme -->
        <link href="css/squeezeBox.ui.css" type="text/css" rel="stylesheet" />

        <!-- Use the CSS below when not using a jQuery UI theme -->
        <link href="css/squeezeBox.css" type="text/css" rel="stylesheet" />

        <!-- jQuery -->
        <script src="http://ajax.googleapis.com/ajax/libs/jquery/1.4/jquery.min.js" type="text/javascript"></script>
        <!-- Squeeze Box script -->
        <script src="js/jquery.squeezeBox.js" type="text/javascript"></script>

* **CSS**: ("squeezeBox.ui.css" shown and used with a jQuery UI theme; see "squeezeBox.css" for non-jQuery UI themes)

        /* override ui default margin for accordions */
        .sb-overall-wrapper.ui-accordion .ui-accordion-header {
          margin-top: 0 !important;
        }
        /* chrome - override overflow, because content has scrollbars  */
        .sb-overall-wrapper.ui-accordion .ui-accordion-content {
          overflow: visible;
        }

* **HTML**: Minimal setup; add IDs to the headers (div.header and h3) to use with browser hash marks.

         <div id="squeezeBox">

           <!-- Non-collapsible header -->
           <div class="header">
             <h3>Title</h3>
             <p>Some text.</p>
           </div>

           <h3>Block 1</h3>
           <div>Block 1 content.</div>

           <h3>Block 2</h3>
           <div>Block 2 content.</div>

           <!-- Non-collapsible header -->
           <div class="header">
             <h3>Title</h3>
             <p>Some text.</p>
           </div>

           <h3>Block 3</h3>
           <div>Block 3 content.</div>

           <h3>Block 4</h3>
           <div>Block 4 content.</div>

         </div>

* **Script**: (default settings shown)

        $(window).load(function(){
          $('#squeezeBox').squeezeBox({
            blockHeaders  : 'div.header',           // Class name for non-collapsible headers
            headers       : 'h1, h2, h3',           // Collapsible headers
            fixedClass    : 'fixed',                // Class added when the header becomes fixed in place
            useUI         : true,                   // If true, class names match jQuery UI theme; see squeezeBox.css for full non-UI css.
            enableIcons   : true,                   // If true, accordion arrow images are added
            openIcon      : 'ui-icon-triangle-1-s', // open icon class name - matches jQuery UI icon, but works in non-ui css
            closedIcon    : 'ui-icon-triangle-1-e', // close icon class name - matches jQuery UI icon, but works in non-ui css
            animationTime : 300,                    // Time in milliseconds for scroll animation, only occurs when clicking on a header
            zIndex        : 100                     // z-index added to the headers, so they are above the content
          });
        })

**Options & Details**

* <code>blockHeaders</code>
    * This name should match the class of the non-collapsible headers in the HTML.
    * Non-collapisble headers should be followed by another header (either collapsible or non-collapsible).
    * These non-collapsible headers should not be too tall as they do takes up more space than the collapsible headers. See "To Do" section below for a possible solution.
    * Default is 'div.header'.

* <code>headers</code>
    * This value should contain the HTML targets for the collapsible headers.
    * All collapsible headers must be followed by wrapped content. Divs wrap the content in the example HTML, but any HTML tag can be used.
    * Add an ID to each header to allow using browser hash tags (e.g. id="h2" is the id of the second header in the demo).
    * Default is 'h1, h2, h3'. The demo only uses 'h3', but I included more as the default to show other posibilities.

* <code>fixedClass</code>
    * This class added when the header becomes fixed in place.
    * It is not defined in the CSS, but it available for any styling changes.
    * Default value is 'fixed'.

* <code>useUI</code>
    * If true:
        * Class names matching jQuery UI themes will be added (all classes begin with "ui-").
        * Only two additional css definitions are needed to tweak the squeezeBox styling - see CSS section above.
        * If a UI theme is used, none of the images in the image folder are needed.
    * If false:
        * All class name prefixes are changed to "sb-".
        * See "squeezeBox.css" for all of the non-UI css.
        * Additionally, all images in the image folder will be required, at least for the demo. Modify and replace these images as desired.
    * Default value is true.

* <code>enableIcons</code>
    * If true, accordion arrow images are added. The "openIcon" class will be added to all expanded headers while the "closedIcon" class is added to fixed headers with collapsed content.
    * Default value is true.

* <code>openIcon</code>
    * This icon class name matches a jQuery UI theme icon of an arrow pointing south (down).
    * The non-ui css is set up to use the same class name as the ui icon to make it work in either setting.
    * Default value is 'ui-icon-triangle-1-s'.

* <code>closedIcon</code>
    * This icon class name matches a jQuery UI theme icon of an arrow pointing east (right).
    * The non-ui css is set up to use the same class name as the ui icon to make it work in either setting.
    * Default value is 'ui-icon-triangle-1-e'.

* <code>animationTime</code>
    * This time (in milliseconds) is applied to the scrolling animation which occurs after clicking on a header.
    * Default value is 300 (milliseconds).

* <code>zIndex</code>
    * This value is added to all of the headers, so will appear above the content.
    * Default value is 100.

**Methods**

* **Get**: Get the active header as follows:

        // get jQuery object of active header
        var header = $('#squeezeBox').data('squeezeBox').getHeader();

    * If there is an active header, the code above will return a jQuery object of the header.
    * If no active header is found, nothing is returned (use "header.length" to verify).
    * Use "header[0].id", to get the id of the header or use "header.text()" to get the header title.

* **Set**: You can set the active header multiple ways:
    1. On initial page load, include a window location hash of the target id. The following link take you to the demo page with the "#h2" header active - all headers above it will be collapsed. See "Known Problems &amp; Bugs" section below for more information.

        [http://mottie.github.com/squeezeBox/index.html#h2][3]

    2. Set the active header using its ID:

            // '#b3' is the id of the third block (in the demo)
            $('#squeezeBox').data('squeezeBox').setHeader('#b3');

    3. Set the active header using its index:

            // '#b5' is the sixth header (includes the two non-collapsible headers), zero based index.
            $('#squeezeBox').data('squeezeBox').setHeader(6);

**To Do**

* Add callbacks.
* I plan on making the fixed headers shift up and offscreen as they fill the browser beyond a certain percentage height - something like 50%.

**Known Problems &amp; Bugs**

* Inconsistent results occur when using browser hash tags. I think the problem is the $(window).load() event occurs before the page positioning in some situations/cases/browsers. Right now the headers are updated 600 milliseconds (using a setTimeout) after the window load event which is not ideal.
* Animation in Opera is jumpy. The animation begins from the top of the page each time it is called - I don't know how to resolve this, yet.

**Change Log**

* Version 1.0

    * Initial Release

  [1]: http://store.apple.com/us/compare/mac?page=imac
  [2]: http://mottie.github.com/squeezeBox/index.html
  [3]: http://mottie.github.com/squeezeBox/index.html#h2