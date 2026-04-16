# CRUD Application with Python-Flask and React as frontend


## Part 1: initiating the project

1. Login to GitHub Account
2. Clone the repository (You can fork or upload too)
    - If you go with [codesandbox.io](https://codesandbox.io/) or codespaces (In GitHub): In the repository, create a new Codespaces Environment.
    - If you go with local execution, open the project in your code editor.
4. Run the application

## Part 2: Setting up the frontend static folder

In this project I have edited the `vite.config.js` file and made sure when the build is ran, it created the static output in the dist folder at the same level with the Python app.py. There is no need to modify the `vite.config.js`, unless you change the port number.

```json
import { defineConfig } from 'vite'
import react from '@vitejs/plugin-react'

// https://vitejs.dev/config/
export default defineConfig({
  server:{
    proxy:{
      '/api':"http://127.0.0.1:5000"
    }
  },
  plugins: [react()],
  build: {
    outDir: '../../dist',
  }
})
```

The frontend is based on React library. The steps to run the application are as follows:
 
1. Open the frontend app by using `cd _frontend/users/`
2. Instal dependencies for react.js `npm i`

3. Use `npm run build` to make the dist folder: 

```bash
npm run build
```
4. After you build the app app, you should see the `/dist/` folder in the root directory.


## Part 3: Setting up the backend

The back end of the application is based on Flask library. The steps to run the application are as follows:

1. Initiate the virtual environment:
```bash
python3 -m venv env
```
2. Activate the env:
in Windows run 
```bash
env\source\activate.bat
```
in  Mac OS: 
```bash
source env/bin/activate
```
3. install the needed libraries:
```bash
pip install -r requirements.txt
```
4. run the application:

```bash
python app.py
``` 
(note that python should be installed on your machine, or you can run it on a virtual environment/GitHub Codespaces)

<mark>In order to set up the backend, you need to do the following adjustments:</mark>

Check the python code `app.py`, <mark> make modification to the directories such that flask can find the dist folder of react </mark>.
    - The `template_folder` is the one which the `index.html` file is rendered from
    - The `static_folder` is the host for the static files inside the `/assets`, which `index.html` will need to render.

```python 
app = Flask(__name__, 
      template_folder='./', # here is where you should set the dist directory
      static_folder='./') # here is where you should set the assets directory
```

## Part 4: Setting up the database

In this example, we are using SQLite engine to store the data. SQLite depends on a database file, and in order to generate that file, we need to initiate the app.

The code in the `app.py` file, helps us acheive this goal. **There is no need to edit this code.**

```python
@app.route('/init', methods=['GET'])
def create_database():
    conn = sqlite3.connect('mydatabase.db')
    cursor = conn.cursor()
    try:
        cursor.execute('''CREATE TABLE users
                          (id INTEGER PRIMARY KEY, name TEXT, email TEXT)''')
        conn.commit()
        conn.close()
    except sqlite3.OperationalError:
        conn.close()
        return 'Database already exists!'
    return 'Database created!'
```



**Run the database initiator from the browser:**
in the browser open `/init` to initiate the database. If you are on local machine it will be `http://127.0.0.1:5000/init` and if you are using codespaces, you need to find the link and add `/init` to it.

![codespaces port forwarding](./images/codespaces-port-forwarding.png)

## Part 5: Examine if the web app is functioning normally.

Check if you can see the react application, and make screenshots of the terminal, and the web app itself. Download the repo, and upload it in Canvas.

## Structure of the application

1. backend (app.py)
2. database (mydatabase.db)
3. frontend (_frontend/users)




## <mark>Quick Summary</mark>

> **Two terminals required.** Complete the steps in order.

### Terminal 1 — Build the Frontend
```bash
cd _frontend/users
npm i
npm run build
```

### Terminal 2 — Run the Backend
```bash
python3 -m venv env
source env/bin/activate        # Mac/Linux
env\Scripts\activate.bat       # Windows
pip install -r requirements.txt
python app.py
```

### Browser
| Step | URL | Purpose |
|------|-----|---------|
| 1 | `http://127.0.0.1:5000/init` | Initialize the database |
| 2 | `http://127.0.0.1:5000` | View the app |

> **Using Codespaces?** Replace `http://127.0.0.1:5000` with your forwarded port URL.



## Deliverables

- Full project in Zip Folder, it should contain:
    - It should have screenshots showing functional version (browser screenshot).
    - It should run without any issue, if I run: ```python app.py```
- Screenshot of the running project on Codesandbox, Codespaces or render (IDE/Editor end terminal screenshot)