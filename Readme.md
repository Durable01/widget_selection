# Dataset of the paper
Widget selection dataset stored in folder <code>dataset</code> is used in RQ2, RQ3 and RQ4.
## RQ2\_dataset
<code>dataset\RQ2\_dataset</code> stores GUI pages and widget selection prompts used in RQ2.

There are 4 folders: activity, task, choice, prompt.

### activity
<code> activity </code> stores the context segment of each GUI page. All files are named with <code> "activity" + app.name + "\_" + string(GUI\_index) + "\_.txt" </code>, where <code>app.name</code> is the name of the application, <code>string(GUI\_index)</code> is the index of this GUI page, which is an integer. For example, <code>"activityBlueMoon\_1\_.txt"</code> means the context segment of the first GUI page of BlueMoon app.

### task
<code> task </code> stores all test targets and their descriptions (target segment), where targets related to one GUI page are stored in one file. All files are named with <code> "task" + app.name + "\_" + string(GUI\_index) + "\_.csv" </code>, where <code>app.name</code> is the name of the application, <code>string(GUI\_index)</code> is the index of this GUI page, which is an integer. For example, <code>"taskBlueMoon\_1\_.csv"</code> means test targets of the first GUI page of BlueMoon app.

All task files are three-column tables. Except for the header row, each row represents a test target. The first column provides the test target description, the second column gives the ground truth of the test target (which is an integer representing the option number), and the third column indicates the accessibility category of this widget (also an integer). Here, 0 and 4 denote no accessibility issues, 1 represents information absence, 2 indicates information duplication, and 3 stands for information inconsistency.

### choice
<code> choice </code> stores the choice segment of each GUI page, with each line represents a possibie choice. All files are named with <code> "choice" + app.name + "\_" + string(GUI\_index) + "\_.txt" </code>, where <code>app.name</code> is the name of the application, <code>string(GUI\_index)</code> is the index of this GUI page, which is an integer. For example, <code>"choiceBlueMoon\_1\_.txt"</code> means the choice segment of the first GUI page of BlueMoon app.

### prompt
<code> prompt </code> stores the whole prompt given to LLMs. All files are named with <code> "LLM\_prompt" + app.name + "\_" + string(GUI\_index) + "\_" + string(target\_index) + ".txt" </code>, where <code>app.name</code> is the name of the application, <code>string(GUI\_index)</code> is the index of this GUI page, which is an integer; <code>string(target\_index)</code> is the index of testing target used to compare the ground truth stored in <code>task</code> file, which is also an integer. For example, <code>"LLM\_promptBlueMoon\_1\_0.txt"</code> means the first widget section prompt of the first GUI page of BlueMoon app. 

When using the dataset to test the performance of LLMs, use files in <code>prompt</code> as the prompt of LLMs' input, and use ground truth in <code>task</code>, choice list in <code> choice </code> to compare the result and calculate the hit ratio score.

## RQ3\_dataset
<code>dataset\RQ2\_dataset</code> stores GUI pages and widget selection prompts used in RQ3 and RQ4.

There are 3 folders: prompt, random, choice; with an additional file "list.csv" to store the GUI information complexity of the 15 pages.

### prompt
As mentioned in our paper, we create 28 experimental control pages for each original page, comprising: the original page(page index 0), 15 pages with information absence at three levels (5 pages each at 10%, 40%, and 70%, with page index 1-5, 6-10, 11-15 respectively), 5 with information duplication(page index 16-20), 5 with information inconsistency(page index 21-25), 1 with lightly enhanced widget information(page index 26), and 1 with highly enhanced(page index 27). 
All files are named with <code> "dataset" + app.name + "\_" + string(GUI\_index) + "\_" + string(page\_index) + "\_" + string(target\_index) + ".txt" </code>, where <code> app.name</code> and <code> GUI\_index </code> inherit from the orginal page, <code>page\_index</code> is the page index shown above, <code> target\_index </code> is the index of testing target. Considering that all widgets are tested in RQ3, the index of testing target is also used as the ground truth index.

### random
Files in this folder store the accessibility categories of widgets. There are two types of file, respectively named with <code> "random" + app.name + "\_" + string(GUI\_index) + "\_.csv" </code> and <code> "Trandom" + app.name + "\_" + string(GUI\_index) + "\_.csv" </code>. The second one is the traversal of the first one, which can be used more efficient. 

All Trandom files are tables with 26 rows. Except for the first column serving as serial numbers, each subsequent column sequentially displays the accessibility status of a widget. Apart from the first row serving as the header, each subsequent row presents the accessibility status of a page, with the 25 rows corresponding sequentially to page indices 1 through 25. Accessibility is stored as floating-point numbers, where 0 indicates no accessibility issues, 1 represents information absence, the interval [2,3) denotes information duplication, and 3 signifies information inconsistency. Floating-point numbers are used to facilitate the pairing of information duplication.

### choice
Similar as RQ2, <code> choice </code> stores the choice segment of each GUI page, with each line represents a possibie choice. Files are named with <code> "choice" + app.name + "\_" + string(GUI\_index) + "\_.txt" </code>, where <code>app.name</code> is the name of the application, <code>string(GUI\_index)</code> is the index of this GUI page, which is an integer. Note that information absence experiments have different choice lists as others, for page index from 1 to 15, we use files named with <code> "choice" + app.name + "\_" + string(GUI\_index) + "\_" + string(page\_index) +".txt" </code> instead of normal choice files.

When using the dataset to test the performance of LLMs, use files in <code>prompt</code> as the prompt of LLMs' input, and use accessibility categories in <code>random</code>, choice list in <code> choice </code> to compare the result and calculate the hit ratio score and variability score.

## RQ4_screenshots
15 screenshots of GUI page in RQ3 are stored in this folder. They are used in multi-modal scenarios of RQ4. 

# Code of the paper

For the RQ2 part, two jupyter files <code> RQ2_experiment </code> and <code> RQ2_Statistic </code> are programmed.

<code> RQ2_experiment </code> utilizes open_source LLMs, which are download from Modelscope. Please refer to documents of those model to configure. It uses dataset of RQ2, and output textual logs of LLMs' result, and analyse their selection results to file.

<code> RQ2_Statistic </code> using selection results to calculate the hit ratio and variability of those models, and output the result tables to file.

For the RQ3 and RQ4 part, two jupyter files <code> RQ3_experiment </code> and <code> Get_RQ3_statistic </code> are programmed. Target of those files are similar to those in RQ2.

Except packages used by LLMs, packages below are used in our code:

<code>

    pandas version == 2.1.1
    numpy version == 1.26.0
    
    // Use pip to install as example.
    pip install pandas==2.1.1
    pip install numpy==1.26.0
</code>


