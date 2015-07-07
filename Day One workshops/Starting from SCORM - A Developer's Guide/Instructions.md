
# Starting from SCORM: A Developer's Guide

## Purpose
This session includes an overview of the SCORM to TLA Roadmap as well as a hands-on workshop where an existing SCORM course is updated to track data using xAPI.  In this tutorial you will learn:

   1. the SCORM to xAPI phases and how to determine the best fit for your organization
   2. general information about the xAPI SCORM Profile and its intended use
   3. what the SCORM-to-xAPI wrapper file is, and how to use it to convert SCOs to xAPI-enabled SCOs
   4. examples of new functionality enabled by applying xAPI to a traditional SCORM course

## Step by Step Instructions


### Step 1 - Setup
---
In this step, we’ll examine the SCORM course to be used during this workshop and look at several resources that will be used in this demonstration.

Get the required resources:
   * [SCORM 2004 APIWrapper.js](https://github.com/adlnet/SCORM-to-xAPI-Wrapper/blob/master/SCORM2004/APIWrapper.js) - The new SCORM Wrapper containing integration points for the conversion code
   * [SCORMToXAPIFunctions.js](https://github.com/adlnet/SCORM-to-xAPI-Wrapper/blob/master/SCORMToXAPIFunctions.js) - Contains all of the code required to map SCORM data model elements to xAPI statements
   * [xapiwrapper.min.js](https://github.com/adlnet/xAPIWrapper/blob/master/dist/xapiwrapper.min.js) - Obscures complexities of the xAPI and includes the ADL core verbs
   * [Step 1 - RosesOriginal.zip](add link)
   * [Step 2 - RosesWithInitialXAPIWrapperIntegration.zip](add link)

Extract “Step 1 - RosesOriginal.zip” to a local directory on your computer.  This will be our starting place for the hands-on workshop.  

In the "Step 1 - RosesOriginal".zip" directory structure, examine the following files:

   * imsmanifest.xml for sco locations
   * Assessments/assess_q1.html
   * Introduction_To_Roses/Introduction.html
   * PostTest/Posttest.html

*Workshop Demonstration - SCORM Course in an LMS*

Note: If you have access to an LMS and would like to import your course steps into the LMS, please do so.  This is not required or needed to complete the workshop.


### Step 2 - Replacing and Adding Files
---
In this step, we will add some resources and make simple changes to enable the tracking of many SCORM Data Model elements via the xAPI (in addition to the original SCORM tracking).

First, add the xapiwrapper.min.js file to the scripts directory (/Shared/JavaScript).  This file will be used to abstract the complexity of the xAPI web service components.

Next, add the SCORMToXAPIFunctions.js file to the scripts direcotry (/Shared/JavaScript).  

Next, replace the SCORM course APIWrapper.js file with the SCORM to xAPI Wrapper files (still named APIWrapper.js)

*Workshop Demonstration - No demonstration capable in this step*


### Step 3 - Update SCOs
---
Next, add the following code in the <head> sections of each SCO in your course. SCO launch files can be identified by looking at the imsmanifest.xml file at the root of the SCORM package. Resource elements with adlcp:scormtype set to "sco" should contain the complete list of SCOs in the course. In this solution, each SCO will be an xAPI 'activity' with associated 'statements'. Paste the following code before the <script> tag that references the APIWrapper.js file.

``` javascript
<script type="text/javascript">
  var activity = document.location.protocol + "//" + document.location.host + document.location.pathname;
</script>
<script type="text/javascript" src="../Shared/JavaScript/xapiwrapper.min.js"></script>
<script type="text/javascript" src="../Shared/JavaScript/SCORMToXAPIFunctions.js"></script>
```

  * Be sure that the path in the src attribute above points to the location of the minified xapiwrapper.min.js and SCORMToXAPIFunctions.js file. This location assumes that one directory up from the SCO location, that there is a Shared/JavaScript directory with your JavaScript files.

The complete list of SCO launch files for this step (and the following) are included below:

   * /Assessments/assess_q1.html
   * /Assessments/assess_q2.html
   * /Assessments/assess_q3.html
   * /Assessments/assess_q4.html
   * /Color_Symbolism/Color_Symbolism.html
   * /Dead_Heading/Dead_Heading.html
   * /Hybrids/Rose_Hybrids.html
   * /Introduction_To_Roses/Introduction.html
   * /PostTest/Posttest.html
   * /Pruning/Pruning.html
   * /Shearing/Shearing.html
   * /Styles_Of_Floristry/Floristry.html
   * /What_Is_A_Rose/What_Is_A_Rose.html


### Step 4 - Set Activity IDs
---
When using the code in Step 3 above, Activity IDs will be automatically generated based on the URL of the SCO. This may be LMS-dependent, so it is also possible to manually configure your activity URIs by changing a line of javascript code in each SCO. This will also ensure that your activity IRIs do not change when you import a new copy of the course or include the same course in an additional LMS. 

To manually configure your activity URI's make the following update to the code above:

``` javascript
var activity = <manually configured URI goes here>;
```
Add the appropriate identifier in each SCO.  The following list provides sample activity IDs that can be used for this exercise:

   * http://adlnet.gov/courses/roses/q1
   * http://adlnet.gov/courses/roses/q2
   * http://adlnet.gov/courses/roses/q3
   * http://adlnet.gov/courses/roses/q4
   * http://adlnet.gov/courses/roses/symbolism
   * http://adlnet.gov/courses/roses/deadheading
   * http://adlnet.gov/courses/roses/hybrids
   * http://adlnet.gov/courses/roses/introduction
   * http://adlnet.gov/courses/roses/posttest
   * http://adlnet.gov/courses/roses/pruning
   * http://adlnet.gov/courses/roses/shearing
   * http://adlnet.gov/courses/roses/styles
   * http://adlnet.gov/courses/roses/what


### Step 5 - Configure LRS Information
---
Several configuration values must be set in the updated APIWrapper.js file.  In order for the wrapper to communicate to an LRS and include relevent contextual information, the LRS information, course identifier and LMS home page are required. In "doInitialize()" method in APIWrapper.js, configure the following lines of code as indicated below:

``` javascript
// xAPI Extension
  var config = {
      lrs:{
         endpoint:"https://lrs.adlnet.gov/xapi/",
         user:"adlbootcamp",
         password:"1234"
      },
      courseId:"http://adlnet.gov/courses/roses",
      lmsHomePage:"http://www.mylms.com",
      isScorm2004:true
  };
```
Now the course can be imported into your LMS and used to track a subset of SCORM data via the xAPI.

### Extra Credit
Now that the course is updated to track to an LRS, you can access data historically not available to a SCORM SCO.  For example, you can get all of the scores associated with the post test and show the learner's score vs. the average of the class. 

In the SCORM to xAPI functions file (/Shared/JavaScript/SCORMToXAPIFunctions.js), add a function to get ALL statements from the LRS based on search/query parameters.  The complete function is listed below.

``` javascript
/*****************************************************************************
**
** xAPI Extension
**
** This function is used get all statements via looping through "more" 
** 
** Note: This function uses xAPI natively and does not correspond to
**       SCORM functionality.  
**
*****************************************************************************/
function GetCompleteStatementListFromLRS(search)
{
    var result = ADL.XAPIWrapper.getStatements(search);
    var statements = result.statements;

    while(result.more && result.more !== "")
    {
        var res = ADL.XAPIWrapper.getStatements(null, result.more);
        var stmts = res.statements;

        statements.push.apply(statements, stmts);

        result = res;
    }   

    return statements;
}
```

Then add a function that returns a custom score object that contains data about the score (the average, total number of scores, and total of the scores).   The complete function is listed below.

``` javascript
/*****************************************************************************
**
** xAPI Extension
**
** This function is used get the average score on the exam 
** 
** Note: This function uses xAPI natively and does not correspond to
**       SCORM functionality.  It is not possible to query other learners'
**       data with the SCORM Run-Time API
**
*****************************************************************************/
function getScoreData()
{
   console.log("getScoreData");

   // Set up object for score data
   var scoreStructure = new Object();
   scoreStructure.totalNumberOfScores = 0;
   scoreStructure.totalScores = 0;
   scoreStructure.average = 0;


   var search = ADL.XAPIWrapper.searchParams();
   search['activity'] = activity;
   search['verb'] = ADL.verbs.scored.id;
   
   var statements = GetCompleteStatementListFromLRS(search);

   for (var i=0; i < statements.length; i++)
   {
      console.log("   looping");
      // figure out the average
      if (statements[i].result != undefined)
      {
         console.log("   have a valid result");
         scoreStructure.totalNumberOfScores++;
         scoreStructure.totalScores = scoreStructure.totalScores + statements[i].result.score.scaled;
         console.log("      total number of scores: " + scoreStructure.totalNumberOfScores);
         console.log("      total of scores: " + scoreStructure.totalScores);           
      }
   }  

   scoreStructure.average = scoreStructure.totalScores / scoreStructure.totalNumberOfScores;
   console.log("average: " + scoreStructure.average);

   return scoreStructure;
}
```

Next, update the post test (/PostTest/Posttest.html) to call the new getScoreData() function and display the results.  Add the following code above the “res.innerHTML = message;” line.

``` javascript
  // xAPI Extension
  var scoreStructure = getScoreData();
  message += "<br /><br />";
  message += "<strong>Experience API-Enabled Data:</strong>";
  message += "<br />";
  message += "Average score is: " + scoreStructure.average;
  message += "<br />";
  message += "Total number of scores: " + scoreStructure.totalNumberOfScores;
```


