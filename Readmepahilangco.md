# Introduction
Hello and Welcome this is the FindMyBus app and its a prototype that has two parts that work each other 

Backend - The backend is made of django and the database it uses is SQLite, it serves routes, stops, and bus position, and it host staff dashboard in admin
Mobile app - The app is made with Flutter and Android the user interacting with map, ETA, traffic status, and arrival alerts.

For it to run you must need to get its address first, before anything shows up in the app

Here's what you need
Windows with PowerShell and the py launcher (Python 3)
Flutter SDK (to build the app)
cloudflared (to give your phone a public link to your PC)
An Android phone
Internet access: the backend fetches LA Metro stop data and road routing while it loads


here are the command for both python and cloudflare

Install Python specifically versions above 3.13
Check if you already have it:

```powershell
py --version
```

If it prints a version like Python 3.13.x, you're done. If not, install it:

```powershell
winget install Python.Python.3.13
```

Close and reopen PowerShell afterwards. If winget isn't available, download Python from python.org and tick "Add python.exe to PATH" in the installer.

Install Cludlfare
```powershell
winget install Cloudflare.cloudflared
```

Close and reopen PowerShell, then check it worked:

```powershell
cloudflared --version
```

If that works, you can start the tunnel with the shorter command:

```powershell
cloudflared tunnel --url http://127.0.0.1:8000
```

If the project instructions use the full path (C:\Program Files (x86)\cloudflared\cloudflared.exe) and it says the file isn't found, find where it was installed with:

```powershell
where.exe cloudflared
```

and use that path instead.


For flutter sdk and android sdk setup i highly recommend for you to look for tutorial in web, youtube or ask AI for help since putting it here would be very long and it will be more complicated to follow instructions and a 10gb of storage atleast for android sdk


# Starting the backend
Open the powershell or terminal in your IDE and open the folder that has manage.py file on it then paste this on the terminal powershell if your in IDE while if your in powershell just directly paste this

```powershell
cd "<path to>\findmybus_backend"
```
```powershell
pip install -r requirements.txt
```
```powershell
py manage.py migrate
```
```powershell
$env:DJANGO_ALLOWED_HOSTS="localhost,127.0.0.1,10.0.2.2,.trycloudflare.com"
```
```powershell
$env:DJANGO_CSRF_TRUSTED_ORIGINS="https://*.trycloudflare.com"
```
```powershell
py manage.py runserver 0.0.0.0:8000
```

You should wait until Starting development server at http://0.0.0.0:8000/ has appeared

migrate creates the database db.sqlite3 and loads the three routes it contains
The two $env: lines apply only to that Powershell window. Set them again in a any new window
you can check http://127.0.0.1:8000/dashboard/ but you must create a account first to access the admin do this 

Open the backend and paste this
py manage.py createsuperuser

Then it will ask these things
Username: admin
Email address: (you can leave this blank)
Password: ********
Password (again): ********

You will not see your password once you type in both password and password (again)

if you want to reset your password do this

Paste this in the terminal.

py manage.py changepassword admin

The account are saved in the db.sqlite3 passwords are stored in a hash for security, The passenger doesnt need a login.

Now you login and you can see now the admin dashboard there might be times that the route geometry might took long to allign so after you run the backend it is recommend to wait for atleast 30 seconds - 2 minutes.




# How to connect it with the phone. 

First open a new powershell by clicking the plus icon above for new tab while in your own IDE you must click plus icon in terminal or click new terminal.

Once you created new terminal paste this code.

& "C:\Program Files (x86)\cloudflared\cloudflared.exe" tunnel --url http://127.0.0.1:8000

Once you copy that you must wait until this kind of code shows up " https://something-something-something-something.trycloudflare.com" it mustly shows up in the box where it. 

Look for the box that says Your quick Tunnel has been created The line inside it ending in trycloudflare.com is your link.

Once you got the linage code open the app in your phone go to setting and put the link inside the FindMyBus server address the click Test & Save 

Now you can finally see the app running



# How to build the app

open powershell either in your IDE or directly in your computer and run this command

cd "<path to>\findmybus_flutter_app"
$flutter = "<path to>\flutter\bin\flutter.bat"
& $flutter pub get
& $flutter build apk --release

The build might took long somewhere around 8-15 minutes, and can look like its stuck on a running gradle task assemblerealease and thats find 

once the building is done you can see it an the apk is in build\app\outputs\flutter-apk\app-release.apk 

copy it and send it to your phone via drive or others and click download

Now the download is done you can run it by refering to the How to connect it to the phone and if you cannot find is in the above or click Ctrl+F and copy this statement  "How to connect it with the phone. " and paste this in the search bar

# Navigating the app 

Now that you run the app and done pasting the server address in the settings we can now interact with the app and how it works

Map - it show the routes and the current buses running pinch to zoom and drag to move
Where are you going - its a Search bar and opens it
Bell icon - it opens the notifcation window in the app and shows unread alerts where they have a red badge.
Gear icon - it opens the settings.
Scope button - It will locate you in the map and the app my ask for a location permission the first time.
Bottom sheet - you can drag it up or down and it contains the routes, it will change once you click one of the routes and show the buses currently running on that route.

Overview this is the window where it will greet you and ask which bus are you catching today?
Nearest stop - click it to open  that stops route.
tap the Scope icon button and it will track you on the map and scan the nearest stop on you and its distance.
and the bottom sheet where you can drag up and down shows the list of routes
if you see cant load buses check your server address and backend if its still running then click try again.

Route once you click a route it will show a new sheet where you can click which way you going based on your route and clicking will show the buses that are going in your choosen direction.

you can click the bus stop you selected and you can also choose destination stop to see how many stops that is.

if you click and bus stop you can see when will the next bus are heading to your stop, with a arrival estimates, and if you click notify me when bus is near a pop up will apear above your screen and you can see its estimated arrival time.

you can also customize able to customize your notication for the bus to remind you from 100m to 3km

The ETA might be fluctuating where it will go from 5 mins alll the way to 26 mins since the ETA calculation is based on the bus speed. 

The route might also changes into different colors and they indicate the traffic conditions in that road red is for heavy traffic yellow is for light traffic and blue is for clear traffic or no report.

and if you zoom out in your route thats will you gonna see al the stops in your route and when you lciked at them a green pointer will appear in the map and you can set your stop in that are.

Bus sheet this where you can see the details of the bus

The header shows the bus label and route.
Arrival is the estimate for your stop, with distance and how many stops away is it, it will says "Already passed" indicating that the bus has passed on your set up location.

It will shows its details like its speed, next stop, when it was last updated, and the source 
You can also set alert for a specific bus to arrive on your selected stop location and you can even follow that specific bus on the map by clicking on the button of follow this bus. 

The buses in the map will show different colors green means on time, orange means delayed or deviate, and gray means offline or old position.


# Setting an alert
Open a route, then tap Notify me when a bus is near (or Alert me for this bus on a bus).
Pick the stop and which bus.

Move the Alert distance slider. The sheet shows roughly how much warning that gives in normal traffic. Quick buttons set common distances.

Optionally turn on Also alert me when it is arriving for a second notification at about 150 m.
Tap Start alert.

Search tap the "Where are you going" above and you can type a place , stop or route, for example "Koreatown". Results show matching stops with their distance from you, and routes with their directions.

Now lets go on the admin dashboard (you can't open this on app and you must refer to the steps on starting the backend if you want to access it)
You can see all the buses that are running on all routes and you can see their status like their speed, their route, and if they deviate from the route.

# Troubleshoot tips
Can't open file manage.py - You're in the wrong folder. cd into the one that has manage.py (check with dir manage.py).
Server took too long - Wait about a minute after the first start while road lines build. Make sure the server window is still running.
App can't connect - The tunnel link changed. Paste the new one in Settings and tap Test & save.



# Why we choose LA instead of Davao?
We planned that FindMyBus to be designed for Davao City, but we werent able to access its api and its keys and we did try to find ways to know its api but we were unable since we got rejected of its access so instead we decided to use the LA Metro data.

The davao dcbus api are not public and we cant access its live data, stops, and bus feed. Without those the app could not be tested on a real data.
LA metro publishes its API for public and provide all our needs like stop locations and live bus data, which ables us to test our app with a real time data.

The reason that we choose LA specifically is that i have relatives on there that might be able to help us on testing the app if it really works.

# Tester Feedback
My feedback about the app is that navigation is a bit overwhelming but the app is quite helpful since it will alarm when your bus is about to arrive or pass. Comparing it other bus tracking app here in LA it has the feature like what i said before that it will alarm when your bus is about to arrive or pass but also you can check the traffic of the route you selected which is good and its aproach to be more on the passengers side compare it to like let say LA metro where it serves as a multipurpose where it tracks both buses and trains and you can able to plan your trips i would not say that the app is better than La metro or FindMyBus is bad but i like the way it uses feature that provides convenience to the passenger's side.

The limitation for the is that it uses LA routes and the demo only makes sense to those people from LA and most of it features are not yet to be tested here in the davao roads
