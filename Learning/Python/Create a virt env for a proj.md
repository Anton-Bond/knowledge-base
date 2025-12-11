Create a virtual environment for your Python project to isolate its dependencies from the global Python environmen (on Windows):

1. Navigate to Your Project Directory:
```shell
cd path\to\your\project
```
2. Create a Virtual Environment:
```shell
python -m venv venv
```
3. Activate the Virtual Environment:
- For Command Prompt:
```shell
.\venv\Scripts\activate
```
- For PowerShell:
```shell
.\venv\Scripts\Activate
```
4. Install Dependencies:
```shell
pip install -r requirements.txt
```
5. Verify Installation:
```shell
pip freeze
```
6. Deactivate the Virtual Environment:
```shell
deactivate
```

To reinstall the packages:
```shell
pip install -r requirements.txt --upgrade --force-reinstall
```
