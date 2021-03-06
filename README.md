taskaroo
========

![Stable](http://i4.photobucket.com/albums/y123/Freaklotr4/stage_stable.png)

A Java library that handles execution of tasks. Tasks can be executed sequentially, in parallel, or in any order.

**Features**
- Tasks are completely thread safe
- Tasks can be canceled before they are ran and even while (if supported).
- Tasks can be invoked synchronously (blocks until result is returned) and can have a timeout.
- Tasks can be invoked asynchronously (non-blocking)
- Tasks can be grouped and executed in any order (TaskSet)
- Tasks can be grouped and executed in insertion order (TaskList)
- Tasks can be grouped and executed simultaneously (TaskGroup)
- Tasks can be forked to be ran in a separate context

**Documentation**
- [JavaDoc](http://gh.magnos.org/?r=http://clickermonkey.github.com/Taskaroo/)

**Example**

```java
// A task to search a massive haystack for the first occurrence of needle.
public class SearchTask extends Task<Integer> {
    private String needle, haystack;
    public SearchTask(String needle, String haystack) {
        this.needle = needle;
        this.haystack = haystack;
    }
    protected Integer execute() {
        return haystack.indexOf(needle);
    }
}

// Create a service for performing the tasks.
TaskService service = new TaskService();
service.start();

// Create the search task and set the service which will execute it.
SearchTask task = new SearchTask("o", "Hello World");
task.setHandler(service);
// Wait no longer then a second when synchronously invoking.
task.setTimeout(1000);

// Execute the task asynchronously.
task.async(new TaskListener<Integer>() {
    public void onTaskTimeout(Task<Integer> source) {
        // Task timed out (took more than a second).
    }
    public void onTaskSuccess(Task<Integer> source, Integer result) {
        // Task was a success and no errors were thrown.
    }
    public void onTaskFinish(Task<Integer> source) {
        // Task has completed in some manor.
    }
    public void onTaskError(Task<Integer> source, Throwable error) {
        // Task produced an error (NullPointerException?)
    }
    public void onTaskCancel(Task<Integer> source) {
        // Task cancelled before it ever finished.
    }
});

// Execute task synchronously (could timeout if it takes more than a second).
Integer result = task.sync();

// Stop handling tasks.
service.stop();
```

**Builds**
- [taskaroo-1.0.0.jar](http://gh.magnos.org/?r=https://github.com/ClickerMonkey/Taskaroo/blob/master/build/taskaroo-1.0.0.jar?raw=true)
- [taskaroo-src-1.0.0.jar](http://gh.magnos.org/?r=https://github.com/ClickerMonkey/Taskaroo/blob/master/build/taskaroo-src-1.0.0.jar?raw=true) *- includes source code*
- [taskaroo-all-1.0.0.jar](http://gh.magnos.org/?r=https://github.com/ClickerMonkey/Taskaroo/blob/master/build/taskaroo-1.0.0.jar?raw=true) *- includes all dependencies*
- [taskaroo-all-src-1.0.0.jar](http://gh.magnos.org/?r=https://github.com/ClickerMonkey/Taskaroo/blob/master/build/taskaroo-src-1.0.0.jar?raw=true) *- includes all dependencies and source code*

**Projects using taskaroo:**
- [falcon](http://gh.magnos.org/?r=https://github.com/ClickerMonkey/Falcon)

**Dependencies**
- [surfice](http://gh.magnos.org/?r=https://github.com/ClickerMonkey/Surfice)
- [zource](http://gh.magnos.org/?r=https://github.com/ClickerMonkey/Zource)
- [curity](http://gh.magnos.org/?r=https://github.com/ClickerMonkey/Curity)
- [testility](http://gh.magnos.org/?r=https://github.com/ClickerMonkey/Testility) *for unit tests*

**Testing Examples**
- [Testing/org/magnos/task](http://gh.magnos.org/?r=https://github.com/ClickerMonkey/Taskaroo/tree/master/Testing/org/magnos/task)
