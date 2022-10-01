# login 

This was a pwn challenge worth 289 points. 

The description was
```
Free shell for admins!

Author: joseph#8210
nc 2022.ductf.dev 30025 
```
and we were provided with an executable called login and presumably it's source code login.c. 

So the first move was to look through the source code. 

Something that stood out to me is that they implemented a read_int() function

```c
int read_int() {
    char buf[8];
    read_n_delimited(buf, 8, '\n');
    return atoi(buf);
}
```

Whenever a challenge like this implements a function that could easily be replaced by a standard library function (in this case scanf()) it's logical to assume there 
is a reason. 

The function that actually logs you in was 
```c 
void login() {
    int found = 0;

    char username[USERNAME_LEN];
    printf("Username: ");
    read_n_delimited(username, USERNAME_LEN, '\n');
    for(int i = 0; i < NUM_USERS; i++) {
        if(users[i] != NULL) {
            if(strncmp(users[i]->username, username, USERNAME_LEN) == 0) {
                found = 1;

                if(users[i]->uid == 0x1337) {
                    system("/bin/sh");
                } else {
                    printf("Successfully logged in! uid: 0x%x\n", users[i]->uid);
                }
            }
        }
    }

    if(!found) {
        puts("User not found");
    }
}
```
Here you can see that if uid == 1337 you're taken straight to a shell when you login, hence free shell for admins.

So thats the aim. 

Then there was the add_user() function
```c
void add_user() {
    user_t user = (user_t) malloc(sizeof(user_t));
    users[curr_user_id++ - ADMIN_UID] = user;

    printf("Username length: ");
    size_t len = read_int();
    if(len > USERNAME_LEN) {
        puts("Length too large!");
        exit(1);
    }

    if(!user->uid) {
        user->uid = curr_user_id;
    }
    printf("Username: ");
    read_n_delimited(user->username, len, '\n');
}
```

Here user is only allocated the size of user_t
