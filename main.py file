
import pygame
import sys
import time
import sqlite3

conn = sqlite3.connect('testbase')

conn.execute('CREATE TABLE IF NOT EXISTS employeeId (username VARCHAR PRIMARY KEY, password VARCHAR NOT NULL)')

conn.execute('CREATE TABLE IF NOT EXISTS applications (username VARCHAR, jobid INT NOT NULL)')

conn.execute('CREATE TABLE IF NOT EXISTS employees (username VARCHAR PRIMARY KEY, name VARCHAR NOT NULL, email VARCHAR NOT NULL, city VARCHAR)')

#conn.execute('DROP TABLE jobs')
conn.execute('CREATE TABLE IF NOT EXISTS jobs (jobid INT PRIMARY KEY, company VARCHAR NOT NULL, title VARCHAR NOT NULL, city VARCHAR NOT NULL, salary INT NOT NULL, qualification INT NOT NULL, dept VARCHAR NOT NULL)')

try:
    conn.execute('INSERT INTO jobs VALUES (?, ?, ?, ?, ?, ?, ?)',(3, 'Google', 'Web Designer', 'Chennai', 150000, 2, 'Graphics'))
    conn.execute('INSERT INTO jobs VALUES (?, ?, ?, ?, ?, ?, ?)',(1, 'Deloitte', 'Software Dev', 'Delhi', 100000, 1, 'Tech'))
    conn.execute('INSERT INTO jobs VALUES (?, ?, ?, ?, ?, ?, ?)',(2, 'Netflix', 'Accountant', 'Mumbai', 200000, 3, 'Sales'))
except:
    pass

conn.commit()


pygame.init()

s = time.time()
LENGTH = 454
SCREEN = pygame.display.set_mode((LENGTH + 100, LENGTH))

CLOCK = pygame.time.Clock()
sensitivity = 0
#colours
WHITE = pygame.Color('#FFFFFF')
BLACK = (0,0,0)
DARKGRAY = pygame.Color('#222222')
# Apples and Bomb
RED = pygame.Color('#FF4000')  # Apple
BLUE = pygame.Color('#40FFC0')  # Down
GREEN = pygame.Color('#00C000')  # Up
# Snake
ORANGE = pygame.Color('#FF8000')
CADET = pygame.Color('#5f9ea0')
PINK = pygame.Color('#ee70d3')
DARKRED = pygame.Color('#811a13')
# Background
GRAY = pygame.Color('#454545')
LIGHTBROWN = pygame.Color('#AD9157')
DARKBROWN = pygame.Color('#4F3119')
BLACKBROWN = pygame.Color('#11110F')
VOILET = pygame.Color('#400080')
LIGHTPURPLE = pygame.Color('#CBC3E3')

COLOR_INACTIVE = pygame.Color('lightskyblue3')
COLOR_ACTIVE = pygame.Color('dodgerblue2')

lpurple = pygame.Color('#ffffff')
LBLUE=pygame.Color("#dedcdc")


bg_col=LIGHTBROWN
bb_col=BLACKBROWN
text1_col=WHITE

Pop=False
rate = 40

employeesData = conn.execute('SELECT * FROM employees').fetchall()
ids = conn.execute('SELECT * FROM employeeId').fetchall()
jobsData = conn.execute('SELECT * FROM jobs').fetchall()

username, name, email = None, None, None
auth = False

user = 'Home'

def button(text,x,y,width,height,bg_color=text1_col,x_offset=10,text_size=10,text_col=text1_col,hover_col=BLUE,hover_width=2,mode='n'):
    global buttonSound, sensitivity
    pos = pygame.mouse.get_pos()
    if pos[0] >= x and pos[0] <= x + width and pos[1] >= y and pos[1] <= y + height :
        pygame.draw.rect(SCREEN, hover_col,
                         (x - hover_width, y - hover_width,
                          width + hover_width * 2, height + hover_width * 2))
        if pygame.mouse.get_pressed()[0] and (time.time() - sensitivity) > 0.1:
            sensitivity = time.time()
            return True
    pygame.draw.rect(SCREEN, bg_color, (x, y, width, height))
    show(text, text_col, x + x_offset,
         (y + abs(height - text_size) // 2 - int(text_size / 5)), text_size,
         mode)

def show(msg, color, x, y, size, mode='n'):
    f = r'data\font\design.graffiti.comicsansmsgras.ttf' if mode == 'b' else (
        r'data\font\comici.ttf' if mode == 'i' else
        (r'data\font\comicz.ttf' if mode == 'ib' else r'data\font\COMIC.ttf'))
    score_show = pygame.font.Font(f, size).render(msg, True, color)
    SCREEN.blit(score_show, (x, y))


user_text = ''
password_text = ''

user_active = True
password_active = False
login_active, create_active = False, False

def login():
    global user, user_text, user_active, password_active, login_active, create_active, username, name, email
    global employeesData, ids, auth

    login_active, create_active = True, False

    LENGTH = pygame.display.get_surface().get_width()
    SCREEN.fill(CADET)
    pygame.draw.rect(SCREEN, WHITE, (0, 0, LENGTH, 40))
    show('LOGIN TO YOUR ACCOUNT', ORANGE, 15, 5, 20, 'b')
    user = 'Home' if button('Home',LENGTH - 154,5,100,30,CADET,x_offset=10,text_col=WHITE,text_size=16,hover_col=bb_col,hover_width=1) else user
    pygame.draw.rect(SCREEN, WHITE, (120, 100, LENGTH - 250, 300))

    if button(' ', 145, 165,20,20, CADET if user_active else LIGHTPURPLE,x_offset=20,text_col=WHITE,text_size=16,hover_col=bb_col,hover_width=1):
        user_active, password_active = True, False
    pygame.draw.rect(SCREEN, CADET if user_active else LIGHTPURPLE, (180, 150, LENGTH - 360, 50),3)
    
    if button(' ', 145, 235,20,20, CADET if password_active else LIGHTPURPLE,x_offset=20,text_col=WHITE,text_size=16,hover_col=bb_col,hover_width=1):
        user_active, password_active = False, True
    pygame.draw.rect(SCREEN, CADET if password_active else LIGHTPURPLE, (180, 220, LENGTH - 360, 50),3)

    show(user_text, CADET, 200, 160, 20, 'b')
    show(password_text, CADET, 200, 230, 20, 'b') 

    if button('Login',225,335,100,30,CADET,x_offset=20,text_col=WHITE,text_size=16,hover_col=bb_col,hover_width=1):
        print(ids)
        for i in ids:
            if user_text == i[0] and password_text == i[1]:
                tempData = conn.execute('SELECT * FROM employees WHERE username = ?',(user_text,)).fetchone()
                print(tempData)
                username, name, email = tempData[0], tempData[1], tempData[2]
                auth = True
                user = 'Jobs'

newname_active = True
newuser_active = False
newpassword_active = False
newemail_active = False
newname_text, newuser_text, newpassword_text, newemail_text = '', '', '', ''

def newuser():
    global user, newname_active, newuser_active, newpassword_active, user_active, password_active, login_active, employeesData, ids
    global create_active, newname_text, newuser_text, newpassword_text, newemail_active, newemail_text, username, name, email, auth

    login_active, create_active = False, True

    LENGTH = pygame.display.get_surface().get_width()
    SCREEN.fill(CADET)
    pygame.draw.rect(SCREEN, WHITE, (0, 0, LENGTH, 40))
    show('CREATE ACCOUNT', ORANGE, 15, 5, 20, 'b')
    user = 'Home' if button('Home',LENGTH - 154,5,100,30,CADET,x_offset=10,text_col=WHITE,text_size=16,hover_col=bb_col,hover_width=1) else user
    pygame.draw.rect(SCREEN, WHITE, (80, 60, LENGTH - 160, 365))

    # name 
    if button(' ', 145, 110,20,20, CADET if newname_active else LIGHTPURPLE,x_offset=20,text_col=WHITE,text_size=16,hover_col=bb_col,hover_width=1):
        newname_active, newuser_active, newpassword_active, newemail_active = True, False, False, False
        user_active, password_active = False, False
    pygame.draw.rect(SCREEN, CADET if newname_active else LIGHTPURPLE, (180, 90, LENGTH - 360, 50),3)
    show(newname_text, CADET, 200, 100, 20, 'b')
    
    # email
    if button(' ', 145, 175,20,20, CADET if newemail_active else LIGHTPURPLE,x_offset=20,text_col=WHITE,text_size=16,hover_col=bb_col,hover_width=1):
        newname_active, newuser_active, newpassword_active, newemail_active = False, False, False, True
        user_active, password_active = False, False
    pygame.draw.rect(SCREEN, CADET if newemail_active else LIGHTPURPLE, (180, 160,LENGTH - 360, 50),3)
    show(newemail_text, CADET, 200, 170, 20, 'b')

    # username
    if button(' ', 145, 250,20,20, CADET if newuser_active else LIGHTPURPLE,x_offset=20,text_col=WHITE,text_size=16,hover_col=bb_col,hover_width=1):
        newname_active, newuser_active, newpassword_active, newemail_active = False, True, False, False
    pygame.draw.rect(SCREEN, CADET if newuser_active else LIGHTPURPLE, (180, 230, LENGTH - 360, 50),3)
    show(newuser_text, CADET, 200, 240, 20, 'b')

    # password
    if button(' ', 145, 320,20,20, CADET if newpassword_active else LIGHTPURPLE,x_offset=20,text_col=WHITE,text_size=16,hover_col=bb_col,hover_width=1):
        newname_active, newuser_active, newpassword_active, newemail_active = False, False, True, False
    pygame.draw.rect(SCREEN, CADET if newpassword_active else LIGHTPURPLE, (180, 300, LENGTH - 360, 50),3)
    show(newpassword_text, CADET, 200, 310, 20, 'b')

    if button('Create Account',200,375,170,30,CADET,x_offset=20,text_col=WHITE,text_size=16,hover_col=bb_col,hover_width=1):
        try:
            conn.execute('INSERT INTO employeeId VALUES (?, ?)',(newuser_text, newpassword_text))
            conn.execute('INSERT INTO employees VALUES (?, ?, ?, ?)',(newuser_text, newname_text, newemail_text, None))
            conn.commit()
            print('User created')
            ids = conn.execute('SELECT * FROM employeeId').fetchall()
            employeesData = conn.execute('SELECT * FROM employees').fetchall()
            user = 'Login'
        except:
            pass


def house():
    global user
    LENGTH = pygame.display.get_surface().get_width()
    SCREEN.fill(CADET)
    pygame.draw.rect(SCREEN, WHITE, (0, 0, LENGTH, 40)) 
    pygame.draw.rect(SCREEN, WHITE, (250, 100, 270, 300))
    show('JOBSPIRE', ORANGE, 270, 150, 45,'b')
    show("Get your ",DARKGRAY,35,150,40)
    show("Dream job ",DARKGRAY,35,195,40)
    show("NOW! ",DARKGRAY,35,240,40)
    if button('Log In',280,240,100,30,BLACK,x_offset=10,text_col=WHITE,text_size=16,hover_width=1):
        user = 'Login'
    if button('Create Account',280,280,150,30,BLACK,x_offset=10,text_col=WHITE,text_size=16,hover_width=1):
        user = 'NewUser'
    
sortedSalary, sortedQuali = False, False
elementOnDisplay = 0

def popup():
    global Pop
    LENGTH = pygame.display.get_surface().get_width()
    s = pygame.Surface((LENGTH * 2, LENGTH * 2))
    s.set_colorkey(GRAY)
    s.set_alpha(200)
    SCREEN.blit(s, (0, 0))
    pygame.draw.rect(SCREEN, CADET, (50, 150, 450, 200), 0, 1)
    show("Congratulations!, your application has been sent", BLACK, 70, 170, 15)
    if button('Ok',350,300,100,30,WHITE,text_col=DARKRED,hover_col=WHITE):
        Pop = False


def jobs():
    global user, jobsData, sortedSalary, sortedQuali, username, elementOnDisplay,Pop
    LENGTH = pygame.display.get_surface().get_width()
    SCREEN.fill(CADET)
    pygame.draw.rect(SCREEN, WHITE, (0, 0, LENGTH, 40))
    show('EXPLORE JOBS', ORANGE, 20, 5, 20,'b')
    user = 'Home' if button('Log Out',LENGTH - 154,5,100,30,CADET,x_offset=10,text_col=WHITE,text_size=16,hover_col=bb_col,hover_width=1) else user
    
    pygame.draw.rect(SCREEN, WHITE, (25, 50, 170, 400))
    show("Filters:",DARKGRAY,40,70,25,'b')
    if button('Show All',LENGTH - 500,115,100,30,DARKGRAY,x_offset=10,text_col=WHITE,text_size=16,hover_width=1):
        jobsData = conn.execute('SELECT * FROM jobs').fetchall()
    if button('Sales',LENGTH - 500,150,100,30,DARKGRAY,x_offset=10,text_col=WHITE,text_size=16,hover_width=1):
        jobsData = conn.execute('SELECT * FROM jobs WHERE dept = "Sales"').fetchall()
    if button('Graphics',LENGTH - 500,185,100,30,DARKGRAY,x_offset=10,text_col=WHITE,text_size=16,hover_width=1):
        jobsData = conn.execute('SELECT * FROM jobs WHERE dept = "Graphics"').fetchall()
    if button('Management',LENGTH - 500,220,120,30,DARKGRAY,x_offset=10,text_col=WHITE,text_size=16,hover_width=1):
        jobsData = conn.execute('SELECT * FROM jobs WHERE dept = "Management"').fetchall()
    if button('Tech',LENGTH - 500,255,100,30,DARKGRAY,x_offset=10,text_col=WHITE,text_size=16,hover_width=1):
        jobsData = conn.execute('SELECT * FROM jobs WHERE dept = "Tech"').fetchall()

    h = 0

    nJobsData = []
    size = 2
    for i in range(0, len(jobsData), size):
        nJobsData.append(jobsData[i:i+size])

    try:
        for i in range(len(nJobsData[elementOnDisplay])):
            if nJobsData[elementOnDisplay][i][5] == 1:
                qualitext = 'High School'
            elif nJobsData[elementOnDisplay][i][5] == 2:
                qualitext = 'Undergrad'
            elif nJobsData[elementOnDisplay][i][5] == 3:
                qualitext = 'Postgrad'
            pygame.draw.rect(SCREEN, LBLUE, (220, 80 + h, 320, 150))   
            show(nJobsData[elementOnDisplay][i][2],DARKRED,240,90 + h,25,'b')
            show(f"Qualification: {qualitext} ",BLACK,240,125 + h,17)
            show(f"Company: {nJobsData[elementOnDisplay][i][1]} ",BLACK,240,145 + h,17)
            show(f"Salary: {nJobsData[elementOnDisplay][i][4]} ",BLACK,240,167 + h,17)
            show(f"City: {nJobsData[elementOnDisplay][i][3]}",BLACK,240,189 + h,17)
            if button('APPLY',390,185 + h,120,30,BLACK,x_offset=10,text_col=WHITE,text_size=16,hover_width=1):
                Pop=True
                try:
                    conn.execute('INSERT INTO applications VALUES(?,?)',(username, nJobsData[elementOnDisplay][i][0]))
                    conn.commit()                
                except:
                    pass
            h += 170
    except:
        show("No Jobs To Display",BLACK,240,90,25,'b')
    if not elementOnDisplay + 1 == len(nJobsData):
        if button('Next',420,410,100,30,DARKGRAY,x_offset=10,text_col=WHITE,text_size=16,hover_width=1):
            elementOnDisplay += 1
    if elementOnDisplay > 0:
        if button('Previous',300,410,100,30,DARKGRAY,x_offset=10,text_col=WHITE,text_size=16,hover_width=1):
            elementOnDisplay -= 1
    if Pop:
        popup()


def main():
    global event_list, Text_Val,quitpop, user_text, user_active, password_text, login_active, create_active
    global newname_text, newuser_text, newpassword_text, password_active, newemail_active, newemail_text
    SCREEN.fill(bb_col)
    while True:
        event_list = pygame.event.get()
        if user == 'Home':
            house()
        elif user == 'Login':
            login()
        elif user == 'NewUser':
            newuser()
        elif user == 'Jobs':
            jobs()
        for event in event_list:
            if event.type == pygame.QUIT:
                pygame.quit()
                sys.exit()
            if event.type == pygame.KEYDOWN:
                if login_active == True:
                    if user_active == True:
                        if event.key == pygame.K_BACKSPACE:
                            user_text = user_text[:-1]
                        else:
                            user_text += event.unicode
                    if password_active:
                        if event.key == pygame.K_BACKSPACE:
                            password_text = password_text[:-1]
                        else:
                            password_text += event.unicode 
                elif create_active:
                    if newname_active:
                        if event.key == pygame.K_BACKSPACE:
                            newname_text = newname_text[:-1]
                        else:
                            newname_text += event.unicode
                    if newuser_active:
                        if event.key == pygame.K_BACKSPACE:
                            newuser_text = newuser_text[:-1]
                        else:
                            newuser_text += event.unicode
                    if newpassword_active:
                        if event.key == pygame.K_BACKSPACE:
                            newpassword_text = newpassword_text[:-1]
                        else:
                            newpassword_text += event.unicode
                    if newemail_active:
                        if event.key == pygame.K_BACKSPACE:
                            newemail_text = newemail_text[:-1]
                        else:
                            newemail_text += event.unicode
                    # newname_text, newuser_text, newpassword_text
                
        pygame.display.update()
        CLOCK.tick(rate)

main()
