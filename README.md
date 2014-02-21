# SilverStripe Front-end Coding Guidelines

*This document will*

* Make it easier to peer review
* Create a better workflow
* Create a shared vocabulary
* Be a useful reference

*Remember*

**"Dream BIG"** Think about all the things that are possible or even not possible yet. Don’t feel restricted by technology when thinking about the possibilities. Think first about desirability before viability and feasibility, the solutions that emerge in the end should overlap with what was viable and feasible.

Never lose site of the end result **"What will provide the best user experience"**. The technology used won't necessarily impact the user but the way the technology is executed will. Be thorough and always think from the users point of view.

## CSS and SASS

THREE GUIDING PRINCIPLES FOR CSS

1. Make it manageable
2. Make it modular
3. Make it consistent

## Make CSS manageable

**1. Don’t use !important or IDs in your CSS**

IDs are fine for javascript and html. They are not for styling. Overriding IDs with classes is almost impossible (256 to override in most browsers), which severely limits the use of modular components and modular design philosophy.

Never use !important.  If you do need to use !important (eg: for a javascript override where there is no other option), comment it well and make sure the path is specific to just the place you need it.

Remember that you as a developer may not think of every possible future use of the site,  and the use of either !important or an ID could make future changes harder to implement. If the html does not have the classes you require, add them. 

**2. Use classes appropriately**

You should be able to look at a classname and see 2 things: the component it belongs to, and its function:

    <ul class="menu">
      <li class="menu-item">…</li>
      <li class="menu-item">…</li>
    </ul>



It should be easy to find where styles are coming from. You should then be able to open one main file in order to edit component styles.

**3. CSS should not rely on page structure**

Never write css that looks like this:

    section {
      section {
        article {
          .item { }
        }
      }
    }


The number of surrounding base tags should not matter, and without extensive commenting no one will understand your reasoning for this (including yourself in two years’ time). Any change to the html of a page will break the design. Use classes on tags instead, and only nest to a level that is needed.

**4. Keep media queries with context**

Where possible, keep media queries inline with the component code. This acknowledges that each break-point is as important as any other, and makes it easier to build a complete design across the various supported breakpoints.

SASS:

    .feature{
      	// Regular styles
      	@media (some-size){
      		// media specific overrides for .feature
        }
    }

**5. Class based browser hacks with original context**
Use class based IE overrides, and keep the IE overrides inline with the component code. This makes it easier to maintain IE css when a feature is changed. 

HTML:

    <!--[if lte IE 6 ]><html class="no-js ie6 oldie" lang="$ContentLocale" id="ie6"><![endif]-->
    <!--[if IE 7 ]><html class="no-js ie7 oldie" lang="$ContentLocale" id="ie7"><![endif]-->
    <!--[if IE 8 ]><html class="no-js ie8 oldie" lang="$ContentLocale" id="ie8"><![endif]-->
    <!--[if IE 9]><html class="no-js ie9" id="ie9" lang="en"><![endif]-->
    <!--[if gt IE 9]><!--><html class="no-js" lang="$ContentLocale"><!--<![endif]-->


SASS:

    	.feature{
    		// Regular styles
    		.ie8 &{
    			// IE8 specific overrides for .feature
        }
      }

CSS:

    	.feature {}
    	.ie8 .feature {}



## Make CSS modular

**1. Split CSS into components**

Components can exist within components. For example header and nav-main may be separate components even though nav-main only occurs in header. Each component name must be unique and descriptive.

*"We’re not designing pages, we’re designing systems of components."** **—**[Stephen Ha*y](http://bradfrostweb.com/blog/mobile/bdconf-stephen-hay-presents-responsive-design-workflow/)

The goal of components is easily identifiable, manageable, and reusable code. Management of a component is directly correlated to number of lines, e.g 1000 lines is too much, 500 is pushing it.

While reusing a component across projects seems like a good goal, it's only a bonus. Try not to code with other projects in mind. Components may be used across projects but only as a starting point.

**2. Use multiple classes not single class patterns**

    <a class="btn btn-secondary"></a>

is better than:

    <a class="btn-secondary"></a>

While in the latter the HTML is simpler, it reduces flexibility. We want lots of modifiers in a component to create a range of scenarios. The original example assumes there's only one type of modifier. Once this is not the case the single class pattern breaks.

    <a class="btn btn-secondary btn-large"></a> 

is better than

    <a href="btn-secondary-large"></a>

**3. Pages or page types are not components. **

Do not put classes on the body element or similar in order to style a page. This approach should only be used in rare circumstances. 

If a component looks different on a certain page type, create a css class that explains that difference, and add it to the component.

Good:

    .component.larger

Bad:

    .home-page .component

The .typography class should only be used for typography. If specificity is needed, use a layout based class name (eg .content, .main, or .layout) and leave a comment explaining why the class is there. If possible, only give the specific overrides to the rules that need it (pull them out of any nested sass). This will make it easier to deal with overriding the other styles later, and often prevents an unneeded sass nesting level.

**4. Make CSS contained**

Styles that are reused across multiple components should be defined outside a component. These are called "global styles". Everything else should be contained within a component.

Nesting components will happen so we need to make sure that the outer component doesn't needlessly effect the inner component's style.

## Make CSS consistent

Having consistent rules which each developer abides by means that teams can quickly scale without introducing confusion about how a component works.

**1) Use Mixins sparingly - know when to use @extends or components. **

Mixins can bloat the CSS, consider carefully when you use them. If your mixin is above 5 lines of CSS then it should probably just be a component, or part of a component. Using mixins for rounded-corners, drop-shadows etc is a perfectly good use case. 

A good way to decide between using a mixin, or using a placeholder or a component is whether the mixin would have a variable passed to it. Mixins without variables would be better as components or placeholders.

If you are using Sass, then @extends and placeholders cut down on code bloat. For example: 

    // Placeholder
    %heading {
      //heading styles
    }

    // Styles that use placeholder:
    h1 {
      @extend %heading;
    }

    h2 {
      @extend %heading;
    }

Compiles to: 

    h1, h2 {
      //heading styles
    }

Remember that @extends can not be used within media queries.

**2) Use nesting sparingly.**

If using a css preprocessor don't nest more the 3 times if possible (max 5!). This simplifies resulting css rules and makes specificity easier. It also makes it easier to reuse the parts of a component, or restructure the html of a component at a later stage.

If you wouldn’t write it in css, don’t let it output that way when using a preprocessor.

**3) Follow the style guidelines used in the project**

If you are using a css framework, mimic the framework where possible. If you are working on a project that exists already, follow the general style guidelines that project has followed in regards to spaces vs tabs, camelCase vs underscore vs hyphen et al. Otherwise, try to follow this guide as much as possible. 

**4) Class names should contain only hyphens, no underscores or camelCase.**

Just convention. So long as the css is consistently the same, the separator doesn’t matter. If you have a framework, use the same style of selector used there.

## Frameworks

Work with your framework, not against it. If you are new to the framework, take a look around and find out what it can do. Try to use the components within a framework before creating your own. Even if you know a framework well, always look at it again before making your own component - it might do something that could do half the work for you. Where possible, copy the style conventions of the framework you are using. Try to keep changes to the original framework to a minimum. Removing styles not needed is ok, but any theming should be done within your own component files.

Deciding upon a framework is up to the scrum team and depends on the best interest of the client and the users. Think about maintainability for the long term. How easy it is for other developers outside of your scrum team to become productive when dealing with the code? Is the framework well documented?

**Frameworks used right now:**

<table>
  <tr>
    <td></td>
    <td>IE7</td>
    <td>IE8</td>
    <td>Mobile 1st</td>
    <td>CWP Template integration</td>
  </tr>
  <tr>
    <td>Bootstrap 3.0</td>
    <td></td>
    <td>✓ (with js)</td>
    <td>✓</td>
    <td>https://gitlab.cwp.govt.nz/cwp-themes/default.git 
branch: 3.0.0-wip</td>
  </tr>
  <tr>
    <td>Bootstrap 2.*</td>
    <td>✓</td>
    <td>✓</td>
    <td></td>
    <td>https://gitlab.cwp.govt.nz/cwp-themes/default.git 
branch: 1.0.0</td>
  </tr>
  <tr>
    <td>Gumby</td>
    <td></td>
    <td>✓</td>
    <td></td>
    <td>https://gitlab.cwp.govt.nz/nguyer/cwp-gumby-theme.git
branch:  master
(Not officially supported)</td>
  </tr>
</table>


### Grids

Different frameworks do grids differently. Before you start building the scaffolding of a page, make sure that the grid you plan to use matches your design. Never use negative margins to ‘correct’ a grid. If the design doesn’t match a 12 column grid, use another grid.

## Templates

### General

1. Use html5 versions of html tags whenever it is reasonable to do so. They provide better semantic information than older html
2. Always validate your html. 
3. Always think of the worst case user. Would you be happy using this site if you were them? Would it make sense to someone who has never used it before? 
4. The first H1 attribute on a page should be the page title. If it isn't in the design, use a screen reader accessible hiding technique
5. Use aria and role attributes where applicable. Never use images for text. Design comes second to accessibility, but in most cases there should be an acceptable compromise.
6. Use $FirstLast if only the first and last items in a loop need to be styled. This is more semantic than giving each loop item a unique class. 
7. Use data attributes if you want to feed information to javascript. They are easier than classes, and more specific.
8. Don’t use images for design attributes in your html if it can be done with css.
9. Avoid inline styles. There are two exceptions:

    1. Javascript: If you write it yourself, try to add and remove classes instead of using inline styles. Sometimes this isn’t an option. Remember to clean-up inline styles once you are done with them.

    2. CMS editable images or css. Use as little as it takes to produce the result you need. If the CMS is providing a background image, only specify the background-image in the inline style. Leave other background attributes to the css.

### Structure

**Use as few html tags as possible. **

Your html shouldn’t exist just to serve design features. Sometimes this is unavoidable, but if you keep this rule in mind your html will come out cleaner. 

**Code re-use**

Use a DRY (Don’t repeat yourself) technique. If you have used the same code blocks on more than one page, use an include. If it’s slightly different, think about whether you could pass an extra class to the include: 

    <% include Tiles Context=three-by-three %>
    <% include Tiles Context=two-by-two %>

then in Tile.ss:

    <div class="tiles $Context”></div>

Use component files. If something can be pulled out into a non page specific component, make it an include. Name the file to match the component name, and use css classes to match eg:

    TileNav.ss 
    _tile-nav.scss
    .tile-nav { 
      //styles for this component
    }

This will make it easier to recognise which template files match which scss files.

## Javascript

Refer to the following [JavaScript Coding Conventions](https://github.com/silverstripe/silverstripe-framework/blob/76c809fada921e834bc0578b2f661d111dcec3b3/docs/en/misc/coding-conventions/javascript.md) for more information.

Use JSLint with this at the top of the file:

    /*jslint browser: true, nomen: true,  white: true */

The less familiar you are with JavaScript, the more important it is that you trust JSLint. Get a plugin for your text editor that shows you jslint errors on save. This will save you a lot of pain and heartache, even if it frustrates you at first.

### Boilerplate

Your code should be wrapped in a closure to avoid polluting the global namespace. If you are using, jQuery, you can use the following boilerplate:

    /*jslint browser: true, nomen: true,  white: true */
    
    jQuery(function($) {
    "use strict";
      //Code
    });
    

    Note: This will delay "//Code" execution until DOM ready. You may not need to delay all javascript execution in this way. 

## Coding guidelines

1. if/else/for/while/try statements** always** have braces and always go on multiple lines. 

JavaScript allows an if to be written like this:

    if (*condition*) *statement*;

That form is known to contribute to mistakes in projects where many developers are working on the same code. That is why JSLint expects the use of a block:

    if (*condition*) { 
    
      *statements*; 
    
    }

Experience shows that this form is more resilient.

2. Strict equality checks (===) must be used in favor of abstract equality checks (==). 
3. Comment all functions (using multi-line comments)
4. Separate functions from the events that call them. This helps with unit testing, and makes code more modular and easier to understand.

### Testing

Testing JavaScript really all depends on what framework you are using.

[Backbone Unit Tests and Continuous Integration blog post](http://www.silverstripe.org/backbone-unit-tests-and-continuous-integration/) with a link to David’s [Backbone unit testing](https://github.com/flashbackzoo/bitplate) bootstrap module which uses [Mocha](http://visionmedia.github.io/mocha/).

[Angular JS](http://angularjs.org/) has its own [unit](http://docs.angularjs.org/guide/dev_guide.unit-testing) and [end to end](http://docs.angularjs.org/guide/dev_guide.e2e-testing) testing documentation.

Contemporary JavaScript Unit Testing Boilerplate: [https://github.com/wrumsby/js-testing-boilerplate](https://github.com/wrumsby/js-testing-boilerplate).

## General style guidelines

* { on same line as selector
* Properties on new line, even if it's a single property.
* 1 line between rules
* JS files that aren't minified should be *.js and minified should be *.min.js

* Comment scss component files to say what the component is and examples of where it is used
* Unless there is a reason to do otherwise, @extends come first, @mixins go next, then attribute styles. Attribute modifiers (including responsive) go after attribute styles. Nested attributes go after attribute modifiers

* Recommended (but should not fail peer review)*

* Space between rule and brace (.asd { NOT .asd{)
* Properties in alphabetical order. This helps reduce the chance of double declaring a property and helps readability.
* Space between property and value colon ("display: none" NOT "display:none")


## Project setup

### Folder structure

    themes/
      theme-name/
        css/
        fonts/
        images/
        js/
        sass/
        templates/
        config.rb

### SCSS

* Where possible, use index files to include components rather than importing into a file where other styles are defined.
* CSS components are named _component-file.scss. Files with a name beginning with an underscore will not be compiled as individual css files. 
* In your index stylesheet, @import a _shame.scss file last. This is an out  for developers who need to hotfix an scss issue and don’t know where the fix should go. 
* Feel free to use sub folders within your scss folder to separate out types of component, or types of scss file.
* Make sure styles used in the cms are included in a separate file to styles not needed in the CMS (e.g Typography vs Layout et al)


Git repo [https://github.com/silverstripe-ux/frontend-coding-guidelines](https://github.com/silverstripe-ux/frontend-coding-guidelines)

## Testing

[http://www.webpagetest.org/](http://www.webpagetest.org/)
[http://validator.w3.org/](http://validator.w3.org/)
[http://wave.webaim.org/](http://wave.webaim.org/)
[https://addons.mozilla.org/en-US/firefox/addon/accessibility-evaluation-toolb/](https://addons.mozilla.org/en-US/firefox/addon/accessibility-evaluation-toolb/)
[http://www.totalvalidator.com/](http://www.totalvalidator.com/)

## Internal

### Frontend Tools

Some of the internal tools that are used at SilverStripe. 

* Sublime Text 
* Browsers: Chrome, Firefox, Safari, IE 8 +, iOS Simulator (part of xcode)
* Git: use [SourceTree](http://www.sourcetreeapp.com/) or if you prefer then use Terminal. The github app is a nice gui to use alongside a terminal.
* Adobe Creative Cloud
* [ImageOptim](http://imageoptim.com/) for optimising images for the web
* [MacPorts](http://www.macports.org/) or [HomeBrew](http://brew.sh/) for installing Apache, MySQL etc
* [VirtualBox](https://www.virtualbox.org/) - for testing in different environments 
* [Browserstack](http://www.browserstack.com/) could also be used for testing in different environments
