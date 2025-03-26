---
title: "Multitest-Framework_-Running-one-or-multiple-tests-in-one-or-multiple-executable(s)."
description: ""
toc: false
---

- [Overview](#overview)
- [Known Issues](#known-issues)
- [Architecture](#architecture)
  * [Diagram](#diagram)
  * [Test Collection](#test-collection)
  * [Base attribute variables for test suite classes](#base-attribute-variables-for-test-suite-classes)
  * [No LyTestTools launcher fixtures](#no-lytesttools-launcher-fixtures)
  * [Custom CLI args](#custom-cli-args)
- [Test Examples](#test-examples)
  * [Launcher script and hydra script](#launcher-script-and-hydra-script)
  * [Location of existing examples](#location-of-existing-examples)
- [On-boarding new executable for batched automated tests.](#on-boarding-new-executable-for-batched-automated-tests)
  * [Step 1: Creating the new test module](#step-1--creating-the-new-test-module)
  * [Step 2: Creating the new LyTestTools launcher code](#step-2--creating-the-new-lytesttools-launcher-code)
  * [Step 3: Adding custom logic to multi_test_framework.py](#step-3--adding-custom-logic-to-multi-test-frameworkpy)
  * [Step 4: Creating hydra script](#step-4--creating-hydra-script)
  * [Step 5: Creating test launcher script](#step-5--creating-test-launcher-script)

## Overview
* Code locations:
  * MultiTestSuite: https://github.com/o3de/o3de/blob/development/Tools/LyTestTools/ly_test_tools/o3de/multi_test_framework.py
  * AtomToolsTestSuite: https://github.com/o3de/o3de/blob/development/Tools/LyTestTools/ly_test_tools/o3de/atom_tools_test.py
  * EditorTestSuite: https://github.com/o3de/o3de/blob/development/Tools/LyTestTools/ly_test_tools/o3de/editor_test.py
* This document has been written to describe the functionality of the Multitest Framework and how it is possible to execute one or multiple tests within one or multiple executable applications. For instance, running 10 Editor tests within 1 Editor executable.
* The main use for the Multitest Framework is to save time running tests by reducing the time it takes to setup and teardown executable applications (such as Editor). That's why we run multiple tests in one Editor, otherwise we'd add extra minutes onto our test run creating and tearing down Editors. Removing those setup/teardown minutes is essential to an efficient test run.
## Known Issues
* **Use batched tests:** As time has gone on, users have mostly opted to use the batched tests instead of parallel tests of any kind. This is because introducing parallelization creates an unsatisfactory level of risk where tests will fail more intermittently due to a race condition or something unrelated to the test itself since we're running in parallel (memory leaks, random crashes, etc.). We can explore making parallelization more stable or remove it in the future, but for now we are choosing to leave it as is. If you want to have more stability, we recommend using the batched test option.
* **pytest fixtures:** We have a custom collection process for our batched test run that circumvents parts of the pytest test collection system that utilizes fixtures. For this reason, you may see some strange object inheritance on objects such as how the EditorTestSuite and AtomToolsTestSuite classes inherit from the MultiTestSuite class or you might see a lack of __init__() usage on some objects. These oddities are for this reason: we are circumventing / customizing our pytest collection to avoid any fixture scoping or other mechanisms that prevent us from running batched tests efficiently in our framework. When making updates, be careful for these pitfalls and try to use existing pattern examples in the framework. Hopefully in the future we can clean this framework up even more to be more user friendly, as the current level of complication is not ideal.
## Architecture
### Diagram
![MultiTest Framework](https://i.imgur.com/J8eSomk.png)
* **NOTE:** The _SingleTest_, _SharedTest_, _BatchedTest_, and _ParallelTest_ classes are not included above nor are the EditorTestSuite and AtomToolsTestSuite variants for these classes (which inherit from the base classes in multi_test_framework.py).
* Above is a rough diagram of how the test suite objects are used to run tests. Most of the work is done inside the test suite class itself (_MultiTestSuite_) and new test suite classes (i.e. _AtomToolsTestSuite_) just inherit off the base MultiTestSuite.
* The classes that are NOT part of the MultiTestSuite class (but are still used by the MultiTestSuite class function calls) are: _Result_, _ResultType_, _AbstractTestBase_, _SingleTest_, _SharedTest_, _BatchedTest_, _ParallelTest_, and _TestResultException_.
* Base attribute variables of the base test suite classes drive everything. All of the functions inside `MultiTestSuite` are utilized by `AtomToolsTestSuite` since it inherits from the `MultiTestSuite` class. Examples include `global_extra_cmdline_args`, `use_null_renderer`, `timeout_shared_test`, etc.
### Test Collection
* The `__test__ = False` declaration is used to avoid pytest collection. This is because we have a custom pytest collection setup that works like this:
  * Assume we have a new module named `atom_tools_test.py` that has the `__test__ = False` declaration at the top of its file. We know pytest won't collect this file or anything in it.
  * We create our classes and test code, making sure to create a new `AtomToolsTestSuite(MultiTestSuite)` class object. Inside this `AtomToolsTestSuite` class object we need to create the following function to get our code collected by pytest:
```py
@pytest.mark.parametrize("crash_log_watchdog", [("raise_on_crash", False)])
def pytest_multitest_makeitem(
        collector: _pytest.python.Module, name: str, obj: object) -> AtomToolsTestSuite.MultiTestCollector:
    """
    Enables ly_test_tools._internal.pytest_plugin.multi_testing.pytest_pycollect_makeitem to collect the tests
    defined by this suite.
    This is required for any test suite that inherits from the MultiTestSuite class else the tests won't be
    collected for that suite when using the ly_test_tools.o3de.multi_test_framework module.
    :param collector: Module that serves as the pytest test class collector
    :param name: Name of the parent test class
    :param obj: Module of the test to be run
    :return: AtomToolsTestSuite.MultiTestCollector
    """
    return AtomToolsTestSuite.MultiTestCollector.from_parent(parent=collector, name=name)
```
* This `pytest_multitest_makeitem` function inside `MultiTestSuite` is what enables pytest collection. The hook for this setup is located at `ly_test_tools._internal.pytest_plugin.multi_testing.pytest_pycollect_makeitem` where it searches for the `pytest_multitest_makeitem` function on a given object (in this case `AtomToolsTestSuite`) then knows to use whatever the `pytest_multitest_makeitem` function returns as our test collector.
* This makes our test collector `AtomToolsTestSuite.MultiTestCollector` and now tests can be collected and run by the `MultiTestCollector` class inside of `multi_test_framework.py`. Any attribute defined in `AtomToolsTestSuite` will overwrite any attribute defined by `MultiTestSuite`. Any attribute not defined in `AtomToolsTestSuite` simply inherits from the default `MultiTestSuite` attribute values.
* The `pytest_pycollect_makeitem` code is included here as well, so you can see how it parses for collection of these test suite classes:
```py
def pytest_pycollect_makeitem(collector: _pytest.python.Module, name: str, obj: object) -> _pytest.python.Module:
    """
    Create a custom item collection if the class defines a pytest_multitest_makeitem method. This is used for
    automatically generating test functions with a custom collector.
    Classes that inherit the MultiTestSuite class require a "pytest_multitest_makeitem" method to collect tests.
    :param collector: The Pytest collector
    :param name: Name of the collector
    :param obj: The custom collector, normally a test class object inside the MultiTestSuite class
    :return: Returns the custom collector
    """
    if inspect.isclass(obj):
        for base in obj.__bases__:
            if hasattr(base, "pytest_multitest_makeitem"):
                return base.pytest_multitest_makeitem(collector, name, obj)
```
### Base attribute variables for test suite classes
* Below are all of the base attributes used by the `MultiTestSuite` class:
```py
from ly_test_tools.launchers import launcher_helper


class MultiTestSuite(object):
    """
    Main object used to run the tests.
    The new test suite class you create for your tests should inherit from this base MultiTestSuite class.
    """
    # When this object is inherited, add any custom attributes as needed.
    # Extra cmdline arguments to supply for every executable instance for this test suite
    global_extra_cmdline_args = ["-BatchMode", "-autotest_mode"]
    # Tests usually run with no renderer, however some tests require a renderer and will disable this
    use_null_renderer = True
    # Maximum time in seconds for a single executable to stay open across the set of shared tests
    timeout_shared_test = 300
    # Name of the executable's log file.
    log_name = ""
    # Executable name to look for if the test is an Atom Tools test, leave blank if not an Atom Tools test.
    atom_tools_executable_name = ""
    # Maximum time (seconds) for waiting for a crash file to finish being dumped to disk
    _timeout_crash_log = 20
    # Return code for test failure
    _test_fail_retcode = 0xF
    # Test class to use for single test collection
    _single_test_class = SingleTest
    # Test class to use for shared test collection
    _shared_test_class = SharedTest
```
* And here is an example of the inheriting `EditorTestSuite` class updating the base attribute variables so that the `multi_test_framework.py` code can be used with the `EditorTestSuite` base attribute variables instead of the `MultiTestSuite` base attribute variables:
```py
from ly_test_tools.launchers import launcher_helper


class EditorTestSuite(MultiTestSuite):
    """
    This class defines the values needed in order to execute a batched, parallel, or single Editor test.
    Any new test cases written that inherit from this class can override these values for their newly created class.
    """
    # Extra cmdline arguments to supply for every Editor for this test suite.
    global_extra_cmdline_args = ["-BatchMode", "-autotest_mode"]
    # Tests usually run with no renderer, however some tests require a renderer and will disable this.
    use_null_renderer = True
    # Maximum time in seconds for a single Editor to stay open across the set of shared tests.
    timeout_shared_test = 300
    # Name of the executable's log file.
    log_name = "editor_test.log"
    # Executable name to look for if the test is an Atom Tools test, leave blank if not an Atom Tools test.
    atom_tools_executable_name = ""
    # Maximum time (seconds) for waiting for a crash file to finish being dumped to disk.
    _timeout_crash_log = 20
    # Return code for test failure.
    _test_fail_retcode = 0xF
    # Test class to use for single test collection.
    _single_test_class = EditorSingleTest
    # Test class to use for shared test collection.
    _shared_test_class = EditorSharedTest
```
### No LyTestTools launcher fixtures
* We no longer use fixtures for things like `editor`, `material_editor`, or `generic_launcher`. Instead, `MultiTestSuite` will default to calling `launcher_helper.create_editor` but if a new inheriting class defines the `atom_tools_executable_name` attribute then `MultiTestSuite` will call `self.executable = launcher_helper.create_atom_tools_launcher(workspace, self.atom_tools_executable_name)` instead where the `atom_tools_executable_name` attribute is a string defining the executable to launch (i.e. `MaterialEditor` or `MaterialCanvas`).
  * The `editor`/`material_editor`/etc. fixtures still exist in LyTestTools but they aren't utilized in the `multi_test_framework.py` module any more.
  * The reason for this change has to do with our custom pytest collection that utilizes the `__test__ = False` declaration mechanism on the objects. The fixtures that launch executables such as the ones mentioned do not play nice with this new system of test collection with pytest and so they had to be removed in favor of using the `_executable_function` base attribute variable.
* The final thing to mention is that in our executor functions (i.e. `def _exec_single_test()` & `def _exec_multitest()`) there is some custom logic that changes based on what launcher object type is being used by the current test code.
  * For example, if we have an Editor test then the CLI args for launching the executable are going to be different from the CLI args for a MaterialEditor test. This launcher detection logic is used to determine which values to set in the executor function.
  * Below is an example from `multi_test_framework.py` in the `def _exec_single_test()` function showing where some of this if-else logic exists for different launcher types:
```py
log_path_function = editor_utils.retrieve_log_path
log_content_function = editor_utils.retrieve_editor_log_content
if type(executable) in [WinEditor, LinuxEditor]:
    log_path_function = editor_utils.retrieve_log_path
    log_content_function = editor_utils.retrieve_editor_log_content
    cmdline = ["-runpythontest", test_filename,
               f"-pythontestcase={test_case_name}",
               "-logfile", f"@log@/{log_name}",
               "-project-log-path", log_path_function(run_id, workspace)] + test_cmdline_args
elif type(executable) in [LinuxAtomToolsLauncher, WinAtomToolsLauncher]:
    log_path_function = editor_utils.retrieve_material_editor_log_path
    log_content_function = editor_utils.retrieve_material_editor_log_content
    cmdline = ["-runpythontest", test_filename,
               "-logfile", os.path.join(log_path_function(run_id, workspace), log_name)] + test_cmdline_args
executable.args.extend(cmdline)
executable.start(backupFiles=False, launch_ap=False, configure_settings=False)
```
  * This logic can be extended for any new Launcher type object introduced that also wants to use the Multitest Framework so that in effect automation can be created for any other executable within O3DE. There will be more info on how to do this later in the guide.
### Custom CLI args
While no current examples exist of their usage, there are options built into the `multi_test_framework.py` code (specifically the `MultiTestCollector` class) that check for certain CLI args before running the tests. Below are those CLI arg options, but you can view the code for them here: https://github.com/o3de/o3de/blob/development/Tools/LyTestTools/ly_test_tools/_internal/pytest_plugin/multi_testing.py:
* `--no-test-batch`: Disables test batching completely. Any BatchedTest, EditorBatchedTest, AtomToolsBatchedTest, etc. test class objects will be skipped and not collected if this is enabled.
* `--no-test-parallel`: Disables test parallelization completely. Any ParallelTest, EditorParallelTest, AtomToolsParallelTest, etc. test class objects will be skipped and not collected if this is enabled.
* `--parallel-executables`: Sets the maximum amount of allowed parallel executable instances. The `MultiTestSuite.get_number_parallel_executables()` function checks for this but otherwise uses the default value based on processor count for the current machine. I've included the code for this below:
```py
@staticmethod
def get_number_parallel_executables():
    """
    Number of program executables to run in parallel, this method can be overridden by the user.
    Note: --parallel-executables CLI arg takes precedence over default class settings.
    See ly_test_tools._internal.pytest_plugin.multi_testing.py for full list of CLI options.
    :return: count of parallel program executables to run
    """
    count = 1
    found_processors = os.cpu_count()
    if found_processors:
        # only schedule on half the cores since the application will also run multithreaded
        # also compensates for hyperthreaded/clustered/virtual cores inflating this count
        count = math.floor(found_processors / 2)
        if count < 1:
            count = 1
 
    return count
```
## Test Examples
So now that we have a rough idea of how the Multitest Framework functions, some examples are going to be shown for writing new automated tests.
### Launcher script and hydra script
* Tests are split between a test file/module that executes the actual hydra bindings to interact with the Editor, MaterialEditor, etc. and then a second file/module that handles launching them.

  * A hydra script can be something as simple as below:
```py
class Tests:
    material_editor_launched = (
        "Inspector pane is visible, MaterialEditor launch succeeded",
        "P0: Inspector pane not visible, MaterialEditor launch failed"
    )
 
 
def MaterialEditor_Launched_SuccessfullyLaunched():
    import Atom.atom_utils.atom_tools_utils as atom_tools_utils
 
    from editor_python_test_tools.utils import Report, Tracer, TestHelper
 
    with Tracer() as error_tracer:
        # 1. Verify Material Inspector pane visibility to confirm the MaterialEditor launched.
        atom_tools_utils.set_pane_visibility("Inspector", True)
        Report.result(
            Tests.material_editor_launched,
            atom_tools_utils.is_pane_visible("Inspector") is True)
 
        # 2. Look for errors and asserts.
        TestHelper.wait_for_condition(lambda: error_tracer.has_errors or error_tracer.has_asserts, 1.0)
        for error_info in error_tracer.errors:
            Report.info(f"Error: {error_info.filename} {error_info.function} | {error_info.message}")
        for assert_info in error_tracer.asserts:
            Report.info(f"Assert: {assert_info.filename} {assert_info.function} | {assert_info.message}")
 
 
if __name__ == "__main__":
    from editor_python_test_tools.utils import Report
    Report.start_test(MaterialEditor_Launched_SuccessfullyLaunched)
```
  * The the launcher script would look something like this:
```py
import logging
import pytest

from ly_test_tools.o3de.atom_tools_test import AtomToolsBatchedTest, AtomToolsTestSuite


@pytest.mark.parametrize("project", ["AutomatedTesting"])
@pytest.mark.parametrize("launcher_platform", ['windows_atom_tools'])
class TestMaterialEditor(AtomToolsTestSuite):

    global_extra_cmdline_args = []
    use_null_renderer = False
    log_name = "material_editor_test.log"
    atom_tools_executable_name = "MaterialEditor"
 
    class MaterialEditor_Atom_LaunchMaterialEditor_1(AtomToolsBatchedTest):
 
        from Atom.tests import MaterialEditor_Atom_LaunchMaterialEditor as test_module
 
    class MaterialEditor_Atom_LaunchMaterialEditor_2(AtomToolsBatchedTest):
 
        from Atom.tests import MaterialEditor_Atom_LaunchMaterialEditor as test_module
```
* The objects we've discussed so far largely interact with the launcher script but not the hydra script. Hydra bindings are an entirely separate topic and won't be covered in this guide, but it was important to bring up how they tie in with the launcher scripts.
### Location of existing examples
These examples will demonstrate how simple modifications to the base attributes of the test suite apply to given set of test classes within that test suite.
* Set of `EditorSingleTest` and `AtomToolsSingleTest` classes with their `EditorTestSuite`/`AtomToolsTestSuite` class utilizing `use_null_renderer = False` (GPU tests): https://github.com/o3de/o3de/blob/development/AutomatedTesting/Gem/PythonTests/Atom/TestSuite_Main_GPU.py
* Set of `EditorBatchedTest` classes inside of a `EditorTestSuite` class set with default null renderer batched test run settings: https://github.com/o3de/o3de/blob/development/AutomatedTesting/Gem/PythonTests/Atom/TestSuite_Main_Null_Render_Component_01.py
* No `ParallelTest` examples exist since we aren't utilizing them until the intermittent failure bug can be addressed. They would look mostly the same as the batched test example, but using parallel test classes instead of batched test classes.
* Set of `EditorBatchedTest` classes with some `EditorSingleTest` classes that also utilize skip functionality inside of an `EditorTestSuite` using default attribute variables: https://github.com/o3de/o3de/blob/development/AutomatedTesting/Gem/PythonTests/EditorPythonBindings/TestSuite_Periodic.py
You can create permutations similar to these by just swapping out the type of test classes utilized inside of the test suite class (`SingleTest`, `BatchedTest`, `ParallelTest`, etc.).
## On-boarding new executable for batched automated tests.
Now that we have a firm understanding of the framework and have had some examples to look at, how would we extend this to a new tool or program inside o3de? This section aims to answer that question.

For our example we are going to assume that a fake executable exists named "FakeProgram" and how we would extend functionality to it from multi_test_framework.py
### Step 1: Creating the new test module
First thing we need to make is a new module named fake_program_test.py which will hold our `FakeSingleTest`, `FakeSharedTest`, `FakeParallelTest`, and `FakeBatchedTest` classes. After that, we need to do is create a new test suite class that inherits from `MultiTestSuite`. Then we would need to set any custom attributes the new executable uses on the new test suite class so that our code matches what `FakeProgram` requires.
```py
# Code for `FakeSingleTest`, `FakeSharedTest`, `FakeParallelTest`, and `FakeBatchedTest` classes not included but basically the same as other `multi_test_framework` modules.
 
class FakeProgramTestSuite(MultiTestSuite):
    """
    This class defines the values needed in order to execute a batched, parallel, or single FakeProgram test.
    Any new test cases written that inherit from this class can override these values for their newly created class.
    """
    # Extra cmdline arguments to supply for every FakeProgram for this test suite.
    global_extra_cmdline_args = ["-BatchMode", "-autotest_mode"]
    # Tests usually run with no renderer, however some tests require a renderer and will disable this.
    use_null_renderer = True
    # Maximum time in seconds for a single FakeProgram to stay open across the set of shared tests.
    timeout_shared_test = 300
    # Name of the executable's log file.
    log_name = "fake_program_test.log"
    # Executable name to look for if the test is an Atom Tools test, leave blank if not an Atom Tools test.
    atom_tools_executable_name = ""
    # Executable name to look for if the test is a FakeProgram test, leave blank if not a FakeProgram test.
    fake_program_executable_name = "FakeProgram"
    # Maximum time (seconds) for waiting for a crash file to finish being dumped to disk.
    _timeout_crash_log = 20
    # Return code for test failure.
    _test_fail_retcode = 0xF
    # Test class to use for single test collection.
    _single_test_class = FakeSingleTest
    # Test class to use for shared test collection.
    _shared_test_class = FakeSharedTest
 
    @pytest.mark.parametrize("crash_log_watchdog", [("raise_on_crash", False)])
    def pytest_multitest_makeitem(
            collector: _pytest.python.Module, name: str, obj: object) -> FakeProgramTestSuite.MultiTestCollector:
        """
        Enables ly_test_tools._internal.pytest_plugin.multi_testing.pytest_pycollect_makeitem to collect the tests
        defined by this suite.
        This is required for any test suite that inherits from the MultiTestSuite class else the tests won't be
        collected for that suite when using the ly_test_tools.o3de.multi_test_framework module.
        :param collector: Module that serves as the pytest test class collector
        :param name: Name of the parent test class
        :param obj: Module of the test to be run
        :return: FakeProgramTestSuite.MultiTestCollector
        """
        return FakeProgramTestSuite.MultiTestCollector.from_parent(parent=collector, name=name)
```
Then we need add logic in the various `_exec` functions like what currently exists for the `atom_tools_executable_name` check. Something like this:
```py
    def _run_single_test(self,
                         request: _pytest.fixtures.FixtureRequest,
                         workspace: AbstractWorkspaceManager,
                         collected_test_data: MultiTestSuite.TestData,
                         test_spec: SingleTest) -> None:
        """
        Runs a single test (one executable, one test) with the given specs.
        This function also sets up self.executable for a given program under test.
        :param request: The Pytest Request
        :param workspace: The LyTestTools Workspace object
        :param collected_test_data: The TestData from calling collected_test_data()
        :param test_spec: The test class that should be a subclass of SingleTest
        :return: None
        """
        # Set the self.executable program for Launcher and re-bind our param workspace to it.
        if self.atom_tools_executable_name:  # Atom Tools test.
            self.executable = launcher_helper.create_atom_tools_launcher(workspace, self.atom_tools_executable_name)
        elif self.fake_program_executable_name:  # FakeProgram test.
            self.executable = launcher_helper.create_fake_program(workspace, self.fake_program_executable_name)
        else:  # Editor test.
            self.executable = launcher_helper.create_editor(workspace)
        self.executable.workspace = workspace
```
Repeat for the `_run_batched_tests()`, `_run_parallel_batched_tests()`, and `_run_paralleltests()` functions.

Now we have our test classes in order and add our launcher code in LyTestTools to support the new `FakeProgram` launcher.

### Step 2: Creating the new LyTestTools launcher code
There are several locations that need to be updated to add a new launcher in LyTestTools:
1. Inside https://github.com/o3de/o3de/blob/development/Tools/LyTestTools/ly_test_tools/__init__.py you need to add new launcher options for FakeProgram:
```py
ALL_LAUNCHER_OPTIONS = ['android', 'base', 'linux', 'mac', 'windows', 'windows_editor', 'windows_dedicated', 'windows_generic', 'windows_atom_tools', 'fake_program']
# Rest of the code changes aren't included but you basically just add 'fake_program' to each launcher option it needs to be in.
```
2. You need to add a new function for finding FakeProgram logs inside https://github.com/o3de/o3de/blob/development/Tools/LyTestTools/ly_test_tools/_internal/managers/abstract_resource_locator.py:
```py
def fake_program_log(self):
    """
    Return path to the FakeProgram's log dir using the builds project and platform
    :return: path to FakeProgram.log
    """
    return os.path.join(self.project_log(), "FakeProgram.log")
```
3. Any custom CLI args or options will need to be added to https://github.com/o3de/o3de/blob/development/Tools/LyTestTools/ly_test_tools/_internal/pytest_plugin/multi_testing.py. Our FakeProgram has none, so we aren't updating it.
4. A new Launcher class in https://github.com/o3de/o3de/blob/development/Tools/LyTestTools/ly_test_tools/launchers/platforms/win/launcher.py (you will need to add to other platforms too, only Windows is shown in this example):
```py
class FakeProgramLauncher(WinLauncher):
 
    def __init__(self, build, args=None):
        super(FakeProgramLauncher, self).__init__(build, args)
        self.args.append('--custom-fake-program-arg')
 
    def binary_path(self):
        """
        Return full path to the FakeProgram for this build's configuration and project
        :return: full path to FakeProgram
        """
        assert self.workspace.project is not None
        return os.path.join(self.workspace.paths.build_directory(), "FakeProgram.exe")
```
5. A new launcher helper that will create this FakeProgramLauncher class in https://github.com/o3de/o3de/blob/development/Tools/LyTestTools/ly_test_tools/launchers/launcher_helper.py:
```py
def create_fake_program(workspace, launcher_platform=ly_test_tools.HOST_OS_FAKE_PROGRAM, args=None):
    # type: (workspace_manager.AbstractWorkspaceManager, str, list[str]) -> base_launcher.Launcher
    """
    Create a FakeProgram compatible with the specified workspace.
     FakeProgram is only officially supported on the Windows Platform.
    :param workspace: lumberyard workspace to use
    :param launcher_platform: the platform to target for a launcher (i.e. 'windows_dedicated' for DedicatedWinLauncher)
    :param args: List of arguments to pass to the launcher's 'args' argument during construction
    :return:  FakeProgram instance
    """
    launcher_class = ly_test_tools.LAUNCHERS.get(launcher_platform)
    if not launcher_class:
        log.warning(f"Using default  FakeProgram launcher for '{ly_test_tools.HOST_OS_FAKE_PROGRAM}' "
                    f"as no option is available for '{launcher_platform}'")
        launcher_class = ly_test_tools.LAUNCHERS.get(ly_test_tools.HOST_OS_FAKE_PROGRAM)
    return launcher_class(workspace, args)
```
6. That should be it for now. You can always refer to the PR that added these changes here: https://github.com/o3de/o3de/pull/10722 to see how it can fit into your executable.
### Step 3: Adding custom logic to multi_test_framework.py
Now that we have the `FakeProgramLauncher` and `FakeProgramTestSuite` functionality, we need to make sure `multi_test_framework` has any custom logic inside `_exec_single_test()` and `_exec_multitest()` added now.

For `FakeProgram` we're going to pretend that it needs a custom CLI arg, so we update the code in https://github.com/o3de/o3de/blob/development/Tools/LyTestTools/ly_test_tools/o3de/multi_test_framework.py:
```py
if type(executable) in [WinEditor, LinuxEditor]:
    log_path_function = editor_utils.retrieve_log_path
    log_content_function = editor_utils.retrieve_editor_log_content
    cmdline = ["-runpythontest", test_filename,
               f"-pythontestcase={test_case_name}",
               "-logfile", f"@log@/{log_name}",
               "-project-log-path", log_path_function(run_id, workspace)] + test_cmdline_args
elif type(executable) in [WinMaterialEditor, LinuxMaterialEditor]:
    log_path_function = editor_utils.retrieve_material_editor_log_path
    log_content_function = editor_utils.retrieve_material_editor_log_content
    cmdline = ["-runpythontest", test_filename,
               "-logfile", os.path.join(log_path_function(run_id, workspace), log_name)] + test_cmdline_args
elif type(executable) in [FakeProgramLauncher]:
    cmdline = ["--custom-fake-program-stuff"] + test_cmdline_args
executable.args.extend(cmdline)
executable.start(backupFiles=False, launch_ap=False, configure_settings=False)
```
### Step 4: Creating hydra script
Now we need a hydra script that contains the code that actually interacts with `FakeProgram`:
```py
class Tests:
    fake_program_launched= (
        "Fake Program has launched",
        "P0: Fake Program launch failed"
    )
 
 
def FakeProgram_Launched_SuccessfullyLaunched():
    """
    Summary:
    Tests that the FakeProgram can be launched successfully.
 
    Expected Behavior:
    The  FakeProgram executable can be launched and doesn't cause any crashes or errors.
 
    Test Steps:
    1) Verify FakeProgram is launched.
    2) Look for errors and asserts.
 
    :return: None
    """
 
    import Atom.atom_utils.fake_program_utils as fake_program
 
    from editor_python_test_tools.utils import Report, Tracer, TestHelper
 
    with Tracer() as error_tracer:
        # 1. Verify the FakeProgram has launched.
        launched = fake_program.get_property_value('launched')
        Report.result(
            Tests.fake_program_launched,
            is_launched is True)
 
        # 2. Look for errors and asserts.
        TestHelper.wait_for_condition(lambda: error_tracer.has_errors or error_tracer.has_asserts, 1.0)
        for error_info in error_tracer.errors:
            Report.info(f"Error: {error_info.filename} {error_info.function} | {error_info.message}")
        for assert_info in error_tracer.asserts:
            Report.info(f"Assert: {assert_info.filename} {assert_info.function} | {assert_info.message}")
 
 
if __name__ == "__main__":
    from editor_python_test_tools.utils import Report
    Report.start_test(FakeProgram_Launched_SuccessfullyLaunched)
```
### Step 5: Creating test launcher script
```py
import logging
import pytest
 
from ly_test_tools.o3de.fake_program_test import FakeProgramBatchedTest, FakeProgramTestSuite
 
logger = logging.getLogger(__name__)
 
 
@pytest.mark.parametrize("project", ["AutomatedTesting"])
@pytest.mark.parametrize("launcher_platform", ['fake_program'])
class TestFakeProgram(FakeProgramTestSuite):
 
    class FakeProgram_Atom_LaunchFakeProgram_1(FakeProgramBatchedTest):
 
        from Atom.tests import FakeProgram_Atom_LaunchFakeProgram as test_module
 
    class FakeProgram_Atom_LaunchFakeProgram_2(FakeProgramBatchedTest):
 
```
That should be it. Just register your new test launcher script in your `CMakeLists.txt` file and you're good to go.
