# Widgets

## JobRoll 

JobRoll widget allows displaying filtered list of jobs anywhere on the page. It can be easily configured to display jobs relevant to the current page content by providing proper keywords in search parameters.

It is a javascript layer on top of [job search][job-search-api] public API. Format of input an output data is almost the same as for web service.

### Installation 

Include jobs widgets js file 

    <script src="http://*job-board-url*/static/dist/widgets/jobs.js" ></script>

*note*: `async` make it more efficient rendering, especially on mobile devices, you must use onload handler in if async is used


In the additional inline `<script></script>` or another js file in onload callback create `JobRoll` js object with proper search parameters. 


Do not forget to include custom css rules from https://dj6uj9i1z079.cloudfront.net/jobroll_widget.css. Job roll content is added dynamically to DOM and you have full control over styles. You can start with [basic jobroll css styling][jobroll-css].

### Basic usage

```js


function initJobRoll(){
    // configure widget by providing base url of the board to receive data from
     careerleaf.init({
        baseUrl: 'http://example.com'
     }); 
     
     // define search paramters, all properties are optional
     var search = {
        page_size: 10,
        page: 1,
        companies: [{name: "Richmond Company"}],
        categories: [{name: "Advertising"}]
     }

     // show jobs on the page, first paramter is a container: selector or DOM element
     var jobroll = new careerleaf.jobs.JobRoll('#jobs', search);
}

/// attach to onload handler

/// on document load
document.addEventListener("DOMContentLoaded", initJobRoll);
// or simply  or jQuery.ready 
// window.onload = initJobRoll;


```

See full list of filtering options in [job-search][job-search-api] web service api.
For `job_types, companies and categories` exception will be thrown if matching filtering option cannot be found using information provided.


### Events 

```js 
    jobroll.on('loaded', function(e){
        if(e.data.total == 0){
            $("#jobroll-container").hide();
        }
        console.log('loaded')
    })

```

currently supported events are: 
- `loaded` - triggered when job roll is successfully loaded on the page
- `error` - triggered when service fails to load data 


### Custom Template

It is possible to provide custom template 

```js
    var options = {
        template: function(data){
            // use your favorite template engine 
            return 'this is my custom template, total ' + data.jobs.length + ' jobs found' ;
        }
    }
     var jobroll2 = new careerleaf.jobs.JobRoll('#jobs2', search, options);

```

### Box Template Usage
Remove Comments

```js
// include jobs widgets js file
<script src="http://*job-board-url*/static/dist/widgets/jobs.js"></script>

//define Params
<script type="text/javascript">
//Search parameters (currently it only supports parameters keyword, city and categories, all are optional)
var keyword="Sales Manager";
var city="Swindon";
var categories="Sales";
// Base URL of the board to recieve data from
var website_url="https://*job-board-url*/";
// maps to BOX template instead of the default
var template = "jobsRollBox";
</script>

<div class="main-container">
//Dom element of the container
<div id="jobs">&nbsp;</div>
<style></style>
</div>

//Initialise the jobroll widget
<script>careerleaf.jobs.initJobRoll(website_url,keyword,city,categories,template);</script>

```
careerleaf.jobs.initJobRoll uses `JobRoll` widget

# Services

## JobSearchService 

service used by `JobRoll` widget to run search queries, can be used separately 

methods: 
`JobSearchService.prototype.search` 
accepts `search` object
returns - promise

```js 

var service = new careerleaf.jobs.JobSearchService();
service.search(search)
    .then(function(data){ 
        ///
    })
    .error(function(status){
        /// 
    })


```



[job-search-api]: https://github.com/careerleaf/api/blob/master/sections/job-search.md
[jobroll-css]: https://github.com/careerleaf/api/blob/master/js/jobroll-css.md
