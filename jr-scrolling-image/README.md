# jr-scrolling-image
Explanation of the Code
This code is a jQuery script that sets up and manages a scrolling image gallery with various configurable options like aspect ratio, direction, and pause on hover. Here's a detailed breakdown of the code:

1. Import jQuery
html

<script src="https://code.jquery.com/jquery-3.6.0.min.js"></script>
This line includes the jQuery library from a CDN, allowing the use of jQuery functions and methods in the script.

2. Variable Declarations
javascript

var $ = jQuery,
    html = [],
    prevWidth;
$: Alias for jQuery to use $ instead of jQuery.
html: An array to store HTML content of the scrolling images.
prevWidth: Variable to store the previous window width for resize handling.
3. Function Definitions
a. setAspectRatio

javascript

function setAspectRatio(el) {
    if (el.css('--image-aspect-ratio') && el.css('--image-aspect-ratio').trim() == 'true') {
        el.addClass('image-aspect-ratio');
    } else {
        el.removeClass('image-aspect-ratio');
    }
}
Checks if the CSS custom property --image-aspect-ratio is set to true and adds or removes the image-aspect-ratio class accordingly.
b. getInitialWidth

javascript

function getInitialWidth(el) {
    var width = 0,
        space = parseFloat(el.css('gap'));
    el.find('.elementor-widget').each(function () {
        width += $(this).width() + space;
    });
    return width;
}
Calculates the initial total width of all child elements within the container, including the gap between them.
c. setValues

javascript

function setValues(el, width, indexI, indexJ) {
    var ratio = Math.ceil(el.parent().width() / width),
        total = ratio + 1;
    for (i = 0; i < ratio; i++) {
        el.append(html[indexI][indexJ]);
    }
    el.width(width * total);
    el.css('--total', total);
    el.css('--est-speed', width / 100);
}
Sets the container width and other CSS custom properties based on the calculated width and the parent container's width.
d. setDirection

javascript

function setDirection(el, width) {
    if (el.css('--direction') == -1) {
        el.css('margin-left', -1 * width + 'px');
    }
}
Sets the initial margin-left if the direction is set to -1.
e. setPauseOnHover

javascript

function setPauseOnHover(el) {
    var pauseOnHover = $(window).width() > 767 ? '--pause-on-hover' : '--pause-on-hover-mobile';
    if (el.css(pauseOnHover) && el.css(pauseOnHover).trim() == 'true') {
        el.css('--poh', 'paused');
    } else {
        el.css('--poh', 'running');
    }
}
Sets the --poh CSS custom property to paused or running based on the --pause-on-hover or --pause-on-hover-mobile properties.
4. Document Ready Event
javascript

$(document).ready(function () {
    prevWidth = $(window).width();
    $('.jr-scrolling-image').each(function (indexI) {
        html[indexI] = [];
        $(this).find('.e-con, .e-container').each(function (indexJ) {
            setAspectRatio($(this));
            var width = getInitialWidth($(this));
            html[indexI].push($(this).html());
            if (width) {
                setValues($(this), width, indexI, indexJ);
                setDirection($(this), width);
            }
            setPauseOnHover($(this));
        });
        $(this).addClass('showing');
    });
});
Runs when the DOM is fully loaded.
Initializes prevWidth to the current window width.
Iterates over each .jr-scrolling-image element, setting aspect ratio, calculating initial width, storing HTML content, setting values, direction, and pause on hover properties, then adds the showing class to each container.
5. Window Resize Event
javascript

$(window).on('resize', function () {
    if ($(window).width() == prevWidth) {
        return;
    }
    prevWidth = $(window).width();
    $('.jr-scrolling-image').each(function (indexI) {
        $(this).find('.e-con, .e-container').each(function (indexJ) {
            $(this).empty();
            $(this).append(html[indexI][indexJ]);
            var width = getInitialWidth($(this));
            if (width) {
                setValues($(this), width, indexI, indexJ);
                setDirection($(this), width);
            }
            setPauseOnHover($(this));
        });
    });
});
Runs when the window is resized.
If the window width hasn't changed, the function returns early.
Updates the previous width and reinitializes the scrolling images with the new window size, recalculating widths and setting properties again.
Summary
This script manages a scrolling image gallery by:

Setting aspect ratios.
Calculating initial widths.
Repeating content to fill the container based on its width.
Setting direction and pause on hover properties.
Handling resizing to adjust the gallery dynamically..
