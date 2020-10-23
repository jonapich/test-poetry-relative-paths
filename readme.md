To reproduce the issue on Powershell:

```
(Invoke-WebRequest -Uri https://raw.githubusercontent.com/python-poetry/poetry/master/get-poetry.py -UseBasicParsing).Content | python - --version 1.0.10
git clone https://github.com/jonapich/test-poetry-relative-paths.git
cd test-poetry-relative-paths/test-functools
poetry lock
poetry install
poetry env remove python
(Invoke-WebRequest -Uri https://raw.githubusercontent.com/python-poetry/poetry/master/get-poetry.py -UseBasicParsing).Content | python - --version 1.1.3
poetry lock
poetry install
```

Note: The 1.0.10 install is not necessary for the bug to occur; it's only there to show the breaking change.

Resulting trace:

```
Installing dependencies from lock file

Package operations: 18 installs, 0 updates, 0 removals

  • Installing pyparsing (2.4.7)
  • Installing six (1.15.0)
  • Installing atomicwrites (1.4.0)
  • Installing colorama (0.4.4)
  • Installing attrs (20.2.0)
  • Installing iniconfig (1.1.1)
  • Installing pluggy (0.13.1)
  • Installing packaging (20.4)
  • Installing toml (0.10.1)
  • Installing py (1.9.0)
  • Installing mypy-extensions (0.4.3)
  • Installing pytest (6.1.1)
  • Installing test-lib (0.1.4 C:/Users/user/code/test-poetry-relative-paths/test-functools/test-poetry-relative-paths/test-lib)
  • Installing typing-extensions (3.7.4.3)
  • Installing typed-ast (1.4.1)
  • Installing inflection (0.5.1)
  • Installing test-testing (0.1.4 C:/Users/user/code/test-poetry-relative-paths/test-functools/test-poetry-relative-paths/test-testing)
  • Installing mypy (0.790)

  EnvCommandError

  Command C:\Users\user\AppData\Local\pypoetry\Cache\virtualenvs\test-functools-dloyykXv-py3.8\Scripts\pip.exe install --no-deps -U C:/Users/user/code/test-poetry-relative-paths/test-functools/test-poetry-relative-paths/test-testing errored with the following return code 1, and output:
  Processing c:\users\user\code\test-poetry-relative-paths\test-functools\test-poetry-relative-paths\test-testing
    Installing build dependencies: started
    Installing build dependencies: finished with status 'done'
    Getting requirements to build wheel: started
    Getting requirements to build wheel: finished with status 'done'
      Preparing wheel metadata: started
      Preparing wheel metadata: finished with status 'error'
      ERROR: Command errored out with exit status 1:
       command: 'C:\Users\user\AppData\Local\pypoetry\Cache\virtualenvs\test-functools-dloyykXv-py3.8\Scripts\python.exe' 'C:\Users\user\AppData\Local\pypoetry\Cache\virtualenvs\test-functools-dloyykXv-py3.8\lib\site-packages\pip\_vendor\pep517\_in_process.py' prepare_metadata_for_build_wheel 'C:\Users\user\AppData\Local\Temp\tmp6_drxiht'
           cwd: C:\Users\user\AppData\Local\Temp\pip-req-build-6vl779_r
      Complete output (16 lines):
      Traceback (most recent call last):
        File "C:\Users\user\AppData\Local\pypoetry\Cache\virtualenvs\test-functools-dloyykXv-py3.8\lib\site-packages\pip\_vendor\pep517\_in_process.py", line 280, in <module>
          main()
        File "C:\Users\user\AppData\Local\pypoetry\Cache\virtualenvs\test-functools-dloyykXv-py3.8\lib\site-packages\pip\_vendor\pep517\_in_process.py", line 263, in main
          json_out['return_val'] = hook(**hook_input['kwargs'])
        File "C:\Users\user\AppData\Local\pypoetry\Cache\virtualenvs\test-functools-dloyykXv-py3.8\lib\site-packages\pip\_vendor\pep517\_in_process.py", line 133, in prepare_metadata_for_build_wheel
          return hook(metadata_directory, config_settings)
        File "C:\Users\user\AppData\Local\Temp\pip-build-env-8git_pgc\overlay\Lib\site-packages\poetry\core\masonry\api.py", line 34, in prepare_metadata_for_build_wheel
          poetry = Factory().create_poetry(Path(".").resolve())
        File "C:\Users\user\AppData\Local\Temp\pip-build-env-8git_pgc\overlay\Lib\site-packages\poetry\core\factory.py", line 91, in create_poetry
          self.create_dependency(name, constraint, root_dir=package.root_dir)
        File "C:\Users\user\AppData\Local\Temp\pip-build-env-8git_pgc\overlay\Lib\site-packages\poetry\core\factory.py", line 242, in create_dependency
          dependency = DirectoryDependency(
        File "C:\Users\user\AppData\Local\Temp\pip-build-env-8git_pgc\overlay\Lib\site-packages\poetry\core\packages\directory_dependency.py", line 36, in __init__
          raise ValueError("Directory {} does not exist".format(self._path))
      ValueError: Directory ..\test-lib does not exist
      ----------------------------------------
  ERROR: Command errored out with exit status 1: 'C:\Users\user\AppData\Local\pypoetry\Cache\virtualenvs\test-functools-dloyykXv-py3.8\Scripts\python.exe' 'C:\Users\user\AppData\Local\pypoetry\Cache\virtualenvs\test-functools-dloyykXv-py3.8\lib\site-packages\pip\_vendor\pep517\_in_process.py' prepare_metadata_for_build_wheel 'C:\Users\user\AppData\Local\Temp\tmp6_drxiht' Check the logs for full command output.
  WARNING: You are using pip version 20.2.2; however, version 20.2.4 is available.
  You should consider upgrading via the 'C:\Users\user\AppData\Local\pypoetry\Cache\virtualenvs\test-functools-dloyykXv-py3.8\Scripts\python.exe -m pip install --upgrade pip' command.


  at ~\.poetry\lib\poetry\utils\env.py:948 in _run
       944│                 output = subprocess.check_output(
       945│                     cmd, stderr=subprocess.STDOUT, **kwargs
       946│                 )
       947│         except CalledProcessError as e:
    →  948│             raise EnvCommandError(e, input=input_)
       949│
       950│         return decode(output)
       951│
       952│     def execute(self, bin, *args, **kwargs):
```