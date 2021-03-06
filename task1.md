---
layout: default
---
<div style="text-align: justify;">
 <h1>Σχεδιασμός Συστήματος</h1>
 <p>Όπως φαίνεται και στο block διάγραμμα που ακολουθεί, ο εγκέφαλος όλου του συστήματος είναι το Raspberry Pi. Το πρωτόκολλο το οποίο θα χρησιμοποιήσουμε είναι το <a href="https://en.wikipedia.org/wiki/MQTT" target="_blank">MQTT</a>. 'Ετσι στο Raspberry λειτουργεί ένας <a href="https://en.wikipedia.org/wiki/Message_broker" target="_blank">mqtt broker</a> μέσω του οποίου ανταλλάσσονται τα μηνύματα subscribe – publish από τους πελάτες. Επιπλέον λειτουργεί και το Node-Red που μας παρέχει την λογική και το όμορφο γραφικό περιβάλλον, μέσου του οποίου μπορούμε να αλληλεπιδράσουμε με το σύστημα.</p>
 <p>Για να μπορέσει το Raspberry να δώσει εντολές σε διακόπτες ή να διαβάσει κάποιους αισθητήρες, απαιτείται και κάτι ακόμη. Αυτό είναι ο μικροελεγκτής ο οποίος θα δέχεται οδηγίες από το Raspberry Pi και θα βγάζει ή θα δέχεται τα κατάλληλα σήματα ώστε να επικοινωνεί με τις διάφορες συσκευές του σπιτιού.</p>
 <p>Για να υπάρχει επικοινωνία χωρίς καλωδιακή υποδομή, θα χρησιμοποιήσουμε ασύρματη επικοινωνία WiFi. Για τον λόγο αυτό, χρησιμοποιούμε το <a href="https://www.nodemcu.com/index_en.html" target="_blank">module Node-MCU</a> το οποίο έχει τον ισχυρό μικροελεγκτή των 32bit ESP-8266. Εναλλακτικά θα μπορούσαμε να χρησιμοποιήσουμε τον διπύρηνο και ακόμη πιο ισχυρό ESP-32. Μέσα στην Node-MCU υπάρχει ένα πρόγραμμα που λειτουργεί ως πελάτης mqtt και επιπλέον αποκωδικοποιεί τις εντολές και βγάζει τα κατάλληλα σήματα στους ακροδέκτες ή δέχεται δεδομένα σε ψηφιακή ή αναλογική μορφή και τα μεταφέρει στον broker. Όλα τα προηγούμενα προγραμματίζονται σε Micro Python.</p>
 <p>Το Node-MCU εξ' ορισμού έχει εγκατεστιμένη μια έκδοση της LUA. Εμείς σβήνουμε τον διεμηνευτή της LUA και βάζουμε την Micro Python.</p>
 <p>Επειδή το Node-MCU έχει περιορισμένο αριθμό ακίδων, χρησιμοποιούμε και ένα Arduino Nano το οποίο συνδέεται με το Node-MCU μέσω του διαύλου I2C και λειτουργεί ως μονάδα επέκτασης θυρών (port expander). Το arduino μας δίνει επιπλέον 20 ακροδέκτες.</p>
 <center>
 <a href="{{ "/assets/images/block_diagram.png" | relative_url }}" onclick="return hs.expand(this)" class="highslide" target="_self">
   <img src="{{ "/assets/images/block_diagram_small.png" | relative_url }}" alt="Μεγένθυση" title="Μεγένθυση" style="float: center; margin: 5px; border: 1px solid #000000; width: 300px;">
 </a>
 </center>
 <p>Οι ακροδέκτες αυτοί μπορούν να αλλάξουν χρήση και μπορεί να είναι:</p>
  <ul>
   <li>Ψηφιακές είσοδοι (18)</li>
   <li>Ψηφιακές έξοδοι (18)</li>
   <li>Αναλογικές είσοδοι (8)</li>
   <li>Αναλογικές έξοδοι PWM (6)</li>
   <li>Servo Motor (18)</li>
  </ul>
 <p>Ακολουθούν οι τύποι των ακροδεκτών χρωματισμένοι σύμφωνα με τον τύπο εξαρτημάτων της <a href="{{ "/assets/images/maketa_nodim_katopsi1.png" | relative_url }}" target="_blank">μακέτας</a>.</p>
 <center><img src="{{ "/assets/images/signal_types.png" | relative_url }}" width="220"></center>
 <p>Οι τύποι και οι ακροδέκτες από την πλευρά του Arduino. Οι αριθμοί αφορούν τον αριθμό εξαρτήματος πάνω στη <a href="{{ "/assets/images/maketa_nodim_katopsi1.png" | relative_url }}" target="_blank">μακέτα</a>. Υπάρχει μια αλλαγή μεταξύ του A6 και A7 που είναι αντίστροφα, δηλαδή το A6 πάει στο LDR και το A7 στο φωτοβολταϊκό πάνελ.</p>
 <center><img src="{{ "/assets/images/arduino_pin_connections.png" | relative_url }}" width="380"></center>
 <p>Η μακέτα έχει τον ελάχιστο αριθμό ακροδεκτών με λίγες συσκευές από κάθε είδος ώστε να είναι δυνατή η χρήση της ως εποπτικού μέσου και η πραγματοποίηση εργαστηριακών ασκήσεων. Σε ένα πραγματικό σπίτι οι απαιτήσεις σε ακροδέκτες εισόδου εξόδου είναι πολύ περισσότερες. Έτσι μπορούμε να επεκτείνουμε το σύστημα με τους εξής τρόπους:</p>
 <ol>
   <li>Χρήση των ακίδων GPIO του Raspberry. Μέχρι στιγμής καμία ακίδα του συνδετήρα επέκτασης GPIO του Raspberry, δεν έχει χρησιμοποιηθεί.</li>
   <li>Προσθήκη επιπλέον arduino Nano στον δίαυλο I2C. Για κάθε Arduino έχουμε επιπλέον 20 ακίδες.</li>
   <li>Χρήση κατανεμημένων μονάδων Node MCU (ίσως χωρίς Arduino expander) αρκεί να υπάρχει δίκτυο WiFI. Αυτή η λύση είναι η καλύτερη για παλιά σπίτια στα οποία δεν υπάρχει πρόβλεψη για τερματισμό όλων των καλωδίων σ’ ένα σημείο του κεντρικού πίνακα. Η κάθε μονάδα συνδέεται μέσω WiFi στο Raspberry το οποίο έχει στατική διεύθυνση IP και δημοσιεύει ή δέχεται μηνύματα από τον mosquitto mqtt broker.</li>
 </ol>

 <!--href="{{ "/assets/css/style.css?v=" | append: site.github.build_revision | relative_url }}"-->
 <a href="./index.html">Αρχική</a>
</div>
