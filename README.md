#include<bits/stdc++.h>
#include<conio.h>
#include<iostream>
using namespace std;
const int num_of_patients = 100;
const int num_of_doctors = 10;
int times = 7;
int thisuserindex =0;
int currentuserindex =-1;
int currentdoctorindex =-1;
int thisdoctorindex =0;
struct account{
    string username;
    string password;
};

struct user {
    account useraccount;
    string name;
    int ID ;
};

struct doctor {
    user doctoraccount;
    string specialization;
    string available_time[7];
    bool appointment[7] = { false };
    string patient_names[7];
};
user users[num_of_patients];
doctor doctors[num_of_doctors];

bool signupusers( user users[], int size);
bool signupdoctors( doctor doctors[], int size);
bool findusernamedoctor(string username, doctor users[]);
bool findusernameuser(string username, user users[]);
bool findid(int id, doctor users[]);
string passwordinput();
bool findloginuser(string username, string password, user users[]);
bool findlogindoctor(string username, string password, doctor doctors[]);
bool login (user users[]);
void viewdoctors();
void addtime(doctor doctors[] , int time);
void removetime(doctor doctors[]);
void edittime(doctor doctors[]);
void book(doctor doctors[]);
void viewapp(doctor doctors[], string available_time[]);
void removeapp(doctor doctors[]);
void removeapphistory(doctor doctors[]);
void viewpatientswithappointments();
void displayAvailableDoctorsAtTime(string time);
void edituserusername(user users[]);
void edituserpassword(user users[]);
void editdoctorusername(doctor doctors[]);
void editdoctorpassword(doctor doctors[]);

int main() {
   
    return 0;
}

bool signupusers(user users[], int size) {
    int i = thisuserindex;
    cout << "Enter your Username: \n";
    cin >> users[i].useraccount.username;
    if (findusernameuser(users[i].useraccount.username, users)) {
        cout << "This username already exists \n";
        return false;
    }
    cout << "Enter your password: \n";
     users[i].useraccount.password = passwordinput();
     cout << "Enter your name: \n";
     cin >> users[i].name;
     do {
         users[i].ID = (rand() % 100) + 1;
     } while (findid(users[i].ID, doctors));
     cout << "Your id is:" << users[i].ID << endl;
     thisuserindex++;
    return true;
}

bool signupdoctors( doctor doctors[], int size) {
    int i = thisdoctorindex;
    cout << "Enter your Username: \n";
    cin >> doctors[i].doctoraccount.useraccount.username;
    if (findusernamedoctor(doctors[i].doctoraccount.useraccount.username, doctors)) {
        cout << "This username already exists \n";
        return false;
    }
    cout << "Enter your password: \n";
    doctors[i].doctoraccount.useraccount.password= passwordinput();
    cout << "Enter your name: \n";
    cin >> doctors[i].doctoraccount.name;
    do {
        doctors[i].doctoraccount.ID = (rand() % 10) + 1;
    } while (findid(doctors[i].doctoraccount.ID, doctors));
    cout << "Your id is:" << doctors[i].doctoraccount.ID << endl;
    cout << "Enter your speciality: \n";
    cin >> doctors[i].specialization;
    thisdoctorindex++;
    return true;
}
    
bool findusernamedoctor(string username, doctor doctors[] ) {
    for (int i = 0; i < thisdoctorindex;i++) {
        if (username == doctors[i].doctoraccount.useraccount.username) {
            return true;
        }
    }
    return false;
}

bool findusernameuser(string username, user users[]) {
    for (int i = 0; i < thisuserindex;i++) {
        if (username == users[i].useraccount.username) {
            return true;
        }
    }
    return false;
}

bool findloginuser(string username, string password, user users[]) {
    for (int i = 0; i < thisuserindex;i++) {
        if (username == users[i].useraccount.username && password==users[i].useraccount.password) {
            currentuserindex = i;
            return true;
        }
    }
    return false;
}

bool findlogindoctor(string username, string password, doctor doctors[]) {
    for (int i = 0; i < thisdoctorindex;i++) {
        if (username == doctors[i].doctoraccount.useraccount.username && password == doctors[i].doctoraccount.useraccount.password) {
            currentdoctorindex = i;
            return true;
        }
    }
    return false;
}


string passwordinput() {
    string input = "";
    char ch;
    while ((ch = _getch()) != '\r') {
        cout << "*";
        input += ch;
    }
    cout << endl;
    return input;
}

bool login(user users[]) {
    string username, password;
    cout << "Enter your Username: \n";
    cin >> username;
    cout << "Enter your password: \n";
    password = passwordinput();

    if (findloginuser(username, password, users) || findlogindoctor(username, password, doctors)) {
        cout << "Logged in successfully";
        return true;
    }
    else {
        cout << "Invalid username or password";
        return false;
    }
}

bool findid(int id, doctor users[]) {
    for (int i = 0; i < thisuserindex;i++) {
        if (id == users[i].doctoraccount.ID) {
            return true;
        }
    }
    return false;
}

void viewdoctors(){
    for (int i = 0; i < thisdoctorindex; i++) {
        cout << doctors[i].doctoraccount.name << doctors[i].specialization << endl;
}
}

void addtime(doctor doctors[], int time) {
   // cout << "Number of appointments to take:" << endl;
   // cin >> time;
    for (int i = 0; i < times ; i++) {
        cin >> doctors[i].available_time[i];
    }
}

void removetime(doctor doctors[]) {
    int i;//index of time
    cin >> i;
    i -= 1;
    for (int j = i; j < times - 1; ++j) {
        doctors[currentdoctorindex].available_time[j] = doctors[currentdoctorindex+1].available_time[j+1];
    }
    times--;
}

void edittime(doctor doctors[]) {
    string temp;//time wanted to be put
    int i;//index of time to be edited
    cin >> i;
    cin >> temp;
    doctors[currentdoctorindex].available_time[i] = temp;
}

void book(doctor doctors[]) {
    int doctor_index, slot_index;
    cout << "Enter Doctor Index (0 to " << thisdoctorindex - 1 << "): ";
    cin >> doctor_index;
    cout << "Enter Time Slot Index (1 to 7): ";
    cin >> slot_index;
    slot_index -= 1;
    if (doctors[doctor_index].appointment[slot_index]) {
        cout << "Time slot already booked.\n";
        return;
    }
    string patient_name;
    cout << "Enter your name: ";
    cin >> patient_name;
    doctors[doctor_index].appointment[slot_index] = true;
    doctors[doctor_index].patient_names[slot_index] = patient_name;
    /*cout << "Appointment booked with Dr. " << doctors[doctor_index].doctoraccount.name
        << " at " << doctors[doctor_index].available_time[slot_index] << "\n";*/
}

void viewapp(doctor doctors[],string available_time[]) {
    for (int i = 0; i < thisdoctorindex; i++) {
        for (int j = 0; j < times; j++) {
            if (doctors[i].appointment[j]) {
                cout << doctors[i].available_time[j] << " booked by " << doctors[i].patient_names[j] << endl;
            }
        }
    }
}

void removeapp(doctor doctors[]) {
    int doctor_index, slot_index;
    cin >> doctor_index;
    cin >> slot_index;
    slot_index -= 1;
    doctors[doctor_index].appointment[slot_index] = false;
    doctors[doctor_index].patient_names[slot_index] = "";
}

void removeapphistory(doctor doctors[]) {
    for (int i = 0;i < times;i++) {
        doctors[currentdoctorindex].appointment[i] = false;
        doctors[currentdoctorindex].patient_names[i] = "";
    }
}

void viewpatientswithappointments() {
    for (int i = 0; i < thisdoctorindex; ++i) {
        cout << "Doctor: " << doctors[i].doctoraccount.name << " (" << doctors[i].specialization << ")\n";
        bool hasAppointments = false;
        for (int j = 0; j < times; ++j) {
            if (doctors[i].appointment[j]) {
                cout << "  Time: " << doctors[i].available_time[j]
                    << " | Patient: " << doctors[i].patient_names[j] << "\n";
                hasAppointments = true;
            }
        }
        if (!hasAppointments) {
            cout << "  No appointments.\n";
        }
        cout << "-------------------------\n";
    }
}

void displayAvailableDoctorsAtTime(string time) {
    cin >> time;
    bool found = false;
    cout << "Doctors available at " << time << ":\n";
    for (int i = 0; i < thisdoctorindex; ++i) {
        for (int j = 0; j < times; ++j) {
            if (doctors[i].available_time[j] == time && !doctors[i].appointment[j]) {
                cout << "Dr. " << doctors[i].doctoraccount.name
                    << " | Specialization: " << doctors[i].specialization
                    << " | Time Slot Index: " << j + 1 << "\n";
                found = true;
                break; // No need to check other time slots
            }
        }
    }
    if (!found) {
        cout << "No doctors available at that time.\n";
    }
}

void edituserusername(user users[]) {
    int index = currentuserindex;
    if (index < 0) {
        cout << "No user is currently logged in.\n";
        return;
    }
    string newusername;
    cout << "Enter new username: ";
    cin >> newusername;
    if (findusernameuser(newusername, users)) {
        cout << "This username is already taken.\n";
    }
    else {
        users[index].useraccount.username = newusername;
        cout << "Username updated successfully!\n";
    }
}

void edituserpassword(user users[]) {
    int index = currentuserindex;
    cout << "Enter new password: ";
    users[index].useraccount.password = passwordinput();
    cout << "Password updated successfully!\n";
}

void editdoctorusername(doctor doctors[]) {
    int index = currentdoctorindex;
    if (index < 0) {
        cout << "No doctor is currently logged in.\n";
        return;
    }
    string newusername;
    cout << "Enter new username: ";
    cin >> newusername;
    if (findusernamedoctor(newusername, doctors)) {
        cout << "This username is already taken.\n";
    }
    else {
        doctors[index].doctoraccount.useraccount.username = newusername;
        cout << "Username updated successfully!\n";
    }
}

void editdoctorpassword(doctor doctors[]) {
    int index = currentdoctorindex;
    cout << "Enter new password: ";
    doctors[index].doctoraccount.useraccount.password = passwordinput();
    cout << "Password updated successfully!\n";
}
