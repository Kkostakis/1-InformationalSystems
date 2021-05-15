# 1-InformationalSystems
<<1η υποχρεωτική εργασία στα πληροφοριακά συστήματα>>

Οι παρακάτω οδηγίες αφορούν τον τρόπο εκτέλεσης της 1ης εργασίας στα πληροφοριακά συστήματα και 
αφορούν το λογισμικό kali linux.
Αρχικά, θα πρέπει να έχει εγκαταστήσει ο χρήστης τις εφαρμογές docker, postman, mongodb καθώς και
έναν compiler για την γλώσσα προγραμματισμού python.
Για τις πρώτες δύο εφαρμογές αρκεί μόνο να φτιάξει λογαριασμό.
1ο Βήμα - Eκκίνηση docker, postman, mongodb:
sudo systemctl start docker
sudo systemctl start mongod
(*)και όσο αφορά το postman θα πρέπει αφού το έχετε εγκαταστήσει να πάτε στο directory που έχει εγκατασταθεί
μέσω terminal και να το ξεκινήσετε ως εξής:
./'Postman Agent'
!!!! Προσοχή επιπλέον είναι απαραίτητο να χρησιμοποιηθούν στην python οι βιβλιοθήκες
pymongo, flask και bson.

2o Bήμα - Δημιουργία container και νέας Collection για το mongodb
Για να κάνουμε pull το image από το Docker Hub: 
(sudo) docker pull mongo
Για να κάνουμε deploy το image για πρώτη φορά:
(sudo) docker run -d -p 27017:27017 --name mongodb mongo:4.0.4
Για να ξεκινήσουμε το container(σε περίπτωση που δεν έχει ξεκινήσει)
(sudo) docker ps -a // Επιλέγουμε το ID ή το όνομα του image που θέλουμε να ξεκινήσουμε
και μετά απλά sudo docker start container_id
και αν υπάρχει error απλά θα εκτελέσετε την εντολή
sudo lsof -i //Που δείχνει τις διεργασίες που τρέχουν στο kali και για να τερματήσετε 
την διεργασία του mongodb 
sudo kill -9 uid_του_mongo και μετά πάλι την εντολή sudo docker start container_id

μετά θα πρέπει αφού έχετε ενεργοποιήσει το mongodb να εκτελέσετε την παρακάτω εντολή στο 
directory όπου θα βρίσκονται τα json students.json και users.json 
docker cp students.json mongodb:/students.json
docker exec -it mongodb mongoimport --db=InfoSys --collection=Students --file=students.json
docker cp students.json mongodb:/users.json
docker exec -it mongodb mongoimport --db=InfoSys --collection=Users --file=users.json

και για να μπορείτε να μπείτε στο mongo shell εκτελέστε την εντολή 
(sudo) docker exec -it mongodb mongo
και μέσα στο shell απλά θα χρησιμοποιήσετε τις εντολές
use InfoSys
show Collections
και μετά db.Students.find()
db.Users.find() για να επαληθέυσετε ότι το mongodb έχει δεχτεί τα πάντα όπως πρέπει

3o Bήμα - Ενεργοποίηση του testpr.py και εκτέλεση της εργασίας
στο directory που βρίσκονται τα json και το python script εκτελέστε την εντολή
python3 testpr.py

και τώρα πάμε στο postman 
το οποίο θα ενεργοποιηθεί με τον παραπάνω τρόπο(*)

θα το ανοίξετε σε ένα tab, θα φτιάξετε ένα νέο workspace(+new workspace) το ονομάζετε όπως 
θέλετε και στην συνέχεια θα ανοίξετε μέσα στο workspace ένα νέο tab.
Tώρα, στο νέο tab που έχετε ανοίξει θα παρατηρήσετε ότι υπάρχει μπροστά ένα label με GET
εκεί μπορείτε να το αλλάξετε σε POST, PATCH, DELETE, PUT κλπ ανάλογα με το endpoint που έχετε.
και ακριβώς στα δεξιά υπάρχει ένα κενό πλαίσιο στο οποίο πληκτρολογείτε το url ως εξής
http://localhost:5000/name_of_endpoint ανάλογα με το endpoint που επιθυμείτε να εκτελέσετε
και τέρμα δεξιά υπάρχει ένα κουμπί send το κάνετε κλικ και στέλνει οτιδήποτε έχετε βάλει στο
body του request, header request κλπ.

1ο endpoint -- create-user(εδώ θα πρέπει να έχετε επιλέξει στο postman πως το url αφορά POST)
μέσα στο postman θα δείτε ένα πλαίσιο απο κάτω στο ίδιο tab όπου έχει διάφορες επιλογές εμείς θα 
επιλέξουμε raw -> text -> JSON 
και στο πλαίσιο(body request) αυτό θα γράψετε για το 1ο ερώτημα { "username": "a name"(κατά προτίμηση να 
δώσετε ένα όνομα που υπάρχει στο students.json), "password": "a password" } και μετά απλά πατάτε
send και θα δείτε πως το πρώτο ερώτημα ολοκληρώθηκε

2ο endpoint -- login(εδώ θα πρέπει να έχετε επιλέξει στο postman πως το url αφορά POST)
μέσα στο postman θα δείτε ένα πλαίσιο απο κάτω στο ίδιο tab όπου έχει διάφορες επιλογές εμείς θα 
επιλέξουμε raw -> text -> JSON
και στο πλαίσιο(body request) αυτό θα γράψετε για το 2ο ερώτημα τα ίδια στοιχεία που δώσατε για το 1ο 
γιατί εδώ θέλουμε να δούμε αν ο χρήστης που δημιουργήθηκε προηγουμένως έχει κάνει login
και μετά απλά πατάτε send και θα δείτε πως το δεύτερο ερώτημα ολοκληρώθηκε

3o endpoint -- getStudent(εδώ θα πρέπει να έχετε επιλέξει στο postman πως το url αφορά GET)
Aρχικά, μέσα στον κώδικα της python θα δείτε την εντολή uuid = request.headers.get('authorization'), 
στην συνέχεια θα πάτε στο postman και θα ψάξετε για την επιλογή headers κάνετε κλικ και τώρα
απλά θα πρέπει να πάτε στο πινακάκι με τα keys and values στο πλαίσιο του key θα χρησιμοποιήσετε
ως όνομα το authorization και σαν value το uuid που επιστρέφει το 2o endpoint και θα κάνετε tick
αυτήν την σειρά του πίνακα(στα αριστερά υπάρχει το tick button).
Mέσα στο postman θα δείτε ένα πλαίσιο απο κάτω στο ίδιο tab όπου έχει διάφορες επιλογές εμείς θα 
επιλέξουμε raw -> text -> JSON
και στο πλαίσιο(body request) αυτό θα γράψετε για το 3ο ερώτημα {"email": "an email that exists in students.json"}
μετά απλά πατάτε send και θα δείτε πως το τρίτο ερώτημα ολοκληρώθηκε.

4ο endpoint -- getStudents/thirties(εδώ θα πρέπει να έχετε επιλέξει στο postman πως το url αφορά GET)
Aρχικά, μέσα στον κώδικα της python θα δείτε την εντολή uuid = request.headers.get('authorization'), 
στην συνέχεια θα πάτε στο postman και θα ψάξετε για την επιλογή headers κάνετε κλικ και τώρα
απλά θα πρέπει να πάτε στο πινακάκι με τα keys and values στο πλαίσιο του key θα χρησιμοποιήσετε
ως όνομα το authorization και σαν value το uuid που επιστρέφει το 2o endpoint και θα κάνετε tick
αυτήν την σειρά του πίνακα(στα αριστερά υπάρχει το tick button).
Mέσα στο postman θα δείτε ένα πλαίσιο απο κάτω στο ίδιο tab όπου έχει διάφορες επιλογές εμείς θα 
επιλέξουμε raw -> text -> JSON
και στο πλαίσιο(body request) αυτό θα κάνετε copy paste για το 4ο ερώτημα το students.json
μετά απλά πατάτε send και θα δείτε πως το τέταρτο ερώτημα ολοκληρώθηκε.

5ο endpoint -- getStudents/oldies(εδώ θα πρέπει να έχετε επιλέξει στο postman πως το url αφορά GET) 
Aρχικά, μέσα στον κώδικα της python θα δείτε την εντολή uuid = request.headers.get('authorization'), 
στην συνέχεια θα πάτε στο postman και θα ψάξετε για την επιλογή headers κάνετε κλικ και τώρα
απλά θα πρέπει να πάτε στο πινακάκι με τα keys and values στο πλαίσιο του key θα χρησιμοποιήσετε
ως όνομα το authorization και σαν value το uuid που επιστρέφει το 2o endpoint και θα κάνετε tick
αυτήν την σειρά του πίνακα(στα αριστερά υπάρχει το tick button).
Mέσα στο postman θα δείτε ένα πλαίσιο απο κάτω στο ίδιο tab όπου έχει διάφορες επιλογές εμείς θα 
επιλέξουμε raw -> text -> JSON
και στο πλαίσιο(body request) αυτό θα κάνετε copy paste για το 5ο ερώτημα το students.json
μετά απλά πατάτε send και θα δείτε πως το τέταρτο ερώτημα ολοκληρώθηκε.

6o endpoint -- getStudentAddress(εδώ θα πρέπει να έχετε επιλέξει στο postman πως το url αφορά GET) 
Aρχικά, μέσα στον κώδικα της python θα δείτε την εντολή uuid = request.headers.get('authorization'), 
στην συνέχεια θα πάτε στο postman και θα ψάξετε για την επιλογή headers κάνετε κλικ και τώρα
απλά θα πρέπει να πάτε στο πινακάκι με τα keys and values στο πλαίσιο του key θα χρησιμοποιήσετε
ως όνομα το authorization και σαν value το uuid που επιστρέφει το 2o endpoint και θα κάνετε tick
αυτήν την σειρά του πίνακα(στα αριστερά υπάρχει το tick button).
Mέσα στο postman θα δείτε ένα πλαίσιο απο κάτω στο ίδιο tab όπου έχει διάφορες επιλογές εμείς θα 
επιλέξουμε raw -> text -> JSON
και στο πλαίσιο(body request) για το 6ο ερώτημα {"email": "an email that exists in students.json"}
μετά απλά πατάτε send και θα δείτε πως το έκτο ερώτημα ολοκληρώθηκε.

7ο endpoint -- deleteStudent(εδώ θα πρέπει να έχετε επιλέξει στο postman πως το url αφορά DELETE)
Aρχικά, μέσα στον κώδικα της python θα δείτε την εντολή uuid = request.headers.get('authorization'), 
στην συνέχεια θα πάτε στο postman και θα ψάξετε για την επιλογή headers κάνετε κλικ και τώρα
απλά θα πρέπει να πάτε στο πινακάκι με τα keys and values στο πλαίσιο του key θα χρησιμοποιήσετε
ως όνομα το authorization και σαν value το uuid που επιστρέφει το 2o endpoint και θα κάνετε tick
αυτήν την σειρά του πίνακα(στα αριστερά υπάρχει το tick button).
Mέσα στο postman θα δείτε ένα πλαίσιο απο κάτω στο ίδιο tab όπου έχει διάφορες επιλογές εμείς θα 
επιλέξουμε raw -> text -> JSON
και στο πλαίσιο(body request) για το 7ο ερώτημα {"email": "an email that exists in students.json"}
μετά απλά πατάτε send και θα δείτε πως το έβδομο ερώτημα ολοκληρώθηκε και πως τα στοιχεία για τον
συγκερκιμένο χρήστη έχουν διαγραφεί από το students.json στο mongo shell.

8o endpoint -- addCourses(εδώ θα πρέπει να έχετε επιλέξει στο postman πως το url αφορά PATCH)
Aρχικά, μέσα στον κώδικα της python θα δείτε την εντολή uuid = request.headers.get('authorization'), 
στην συνέχεια θα πάτε στο postman και θα ψάξετε για την επιλογή headers κάνετε κλικ και τώρα
απλά θα πρέπει να πάτε στο πινακάκι με τα keys and values στο πλαίσιο του key θα χρησιμοποιήσετε
ως όνομα το authorization και σαν value το uuid που επιστρέφει το 2o endpoint και θα κάνετε tick
αυτήν την σειρά του πίνακα(στα αριστερά υπάρχει το tick button).
Mέσα στο postman θα δείτε ένα πλαίσιο απο κάτω στο ίδιο tab όπου έχει διάφορες επιλογές εμείς θα 
επιλέξουμε raw -> text -> JSON
και στο πλαίσιο(body request) για το 8ο ερώτημα {"email": "an email that exists in students.json",
"courses":[{"course1":10}, {"course2":4}, {"course3":5 κλπ.}]}
μετά απλά πατάτε send και θα δείτε πως το όγδοο ερώτημα έχει ολοκληρωθεί και πως για τον χρήστη
με το συγκεκριμένο email θα έχουν ανανεωθεί τα μαθήματά του στο students.json στο mongo shell.

9ο endpoint -- getPassedCourses(εδώ θα πρέπει να έχετε επιλέξει στο postman πως το url αφορά GET)
Aρχικά, μέσα στον κώδικα της python θα δείτε την εντολή uuid = request.headers.get('authorization'), 
στην συνέχεια θα πάτε στο postman και θα ψάξετε για την επιλογή headers κάνετε κλικ και τώρα
απλά θα πρέπει να πάτε στο πινακάκι με τα keys and values στο πλαίσιο του key θα χρησιμοποιήσετε
ως όνομα το authorization και σαν value το uuid που επιστρέφει το 2o endpoint και θα κάνετε tick
αυτήν την σειρά του πίνακα(στα αριστερά υπάρχει το tick button).
Mέσα στο postman θα δείτε ένα πλαίσιο απο κάτω στο ίδιο tab όπου έχει διάφορες επιλογές εμείς θα 
επιλέξουμε raw -> text -> JSON
και στο πλαίσιο(body request) για το 9ο ερώτημα {"email": "an email that exists in students.json"}
μετά απλά πατάτε send και θα δείτε πως το τελευταίο ερώτημα ολοκληρώθηκε.




