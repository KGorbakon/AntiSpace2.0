from tkinter import Tk, Canvas, mainloop, NW
from PIL import Image, ImageTk
import time, random

hp = 3
score = 0
extra_ability_waiting_time = 0

window_width = 450
window_height = 700
tk = Tk()
c = Canvas(tk, width=window_width, height=window_height, bg='white')
c.pack()

sky_image = ImageTk.PhotoImage(Image.open('images/sky.jpg').resize((window_width, window_height)))
sky = c.create_image(0, 0, image=sky_image, anchor=NW)

text = c.create_text(225, 100, text = 'AntiSpace', fill = 'white', font=('Courier',50))
text2 = c.create_text(225, 150, text = 'press "Space" to begin', fill = 'white', font=('Courier',20))
text3 = c.create_text(225, 200, text = 'press "E" to use ability', fill = 'white', font=('Courier',20))

ship_image = ImageTk.PhotoImage(Image.open('images/ship.gif').resize((60, 60)))

bullet_image = ImageTk.PhotoImage(Image.open('images/laser.gif').resize((5, 30)))
bullets = []

meteor_image = ImageTk.PhotoImage(Image.open('images/meteor.gif').resize((40, 80)))
meteors = []

ship = c.create_image(195, 600, image=ship_image, anchor=NW)

bonus_heart_image = ImageTk.PhotoImage(Image.open('images/heart.png').resize((60, 60)))
bonus_hearts = []

def starter():
    c.delete(text)
    c.delete(text2)
    c.delete(text3)

    def label_hp():
        global HPLabel
        HPLabel = c.create_text(10, 10, width=300, text="Health Points: " + str(hp), fill='#fff', anchor=NW)

    def label_score():
        global SCORELabel
        SCORELabel = c.create_text(350, 10, width=500, text="Score: " + str(score), fill='#fff', anchor=NW)

    def label_ea():
        global EALabel
        EALabel = c.create_text(150, 10, width=500, text="Extra ability: Ready", fill='#fff', anchor=NW)

    def score_plus():
        global score
        score+=5
        c.delete(SCORELabel)
        label_score()
        c.after(1000, score_plus)

    def get_bullet(shipx, shipy):
        bullet = c.create_image(shipx, shipy, image=bullet_image, anchor=NW)
        bullets.append(bullet)

    def knopki(key):
        global EALabel
        global extra_ability_waiting_time
        shipx, shipy = c.coords(ship)
        if key.char == 'a':
            if shipx > 0:
                c.move(ship, -10, 0)
        if key.char == 'd':
            if shipx < 390:
                c.move(ship, 10, 0)
        if key.char == 'w':
            if shipy > 0:
                c.move(ship, 0, -10)
        if key.char == 's':
            if shipy < 640:
                c.move(ship, 0, 10)
        if key.char == ' ':
                shipx, shipy = c.coords(ship)
                get_bullet(shipx + 55/2, shipy - 20)
        if key.char == 'e':
            if extra_ability_waiting_time == 0:
                extra_ability_waiting_time = 20
                c.delete(EALabel)
                EATimer()
                for i in range(0, 451, 1):
                    get_bullet(i, shipy)        
            
    def EATimer():
        global EALabel
        global extra_ability_waiting_time
        if extra_ability_waiting_time != 0:
            c.after(1000, EATimer_helper)
        else:
            c.delete(EALabel)
            label_ea()

    def EATimer_helper():
        global EALabel
        global extra_ability_waiting_time
        extra_ability_waiting_time -= 1
        c.delete(EALabel)
        EALabel = c.create_text(150, 10, width=300, text="Extra ability ready in: " + str(extra_ability_waiting_time), fill='#fff', anchor=NW)
        EATimer()

    def add_bonus_heart():
        bonus_heart = c.create_image(random.randint(0,390), random.randint(0, 240), image=bonus_heart_image, anchor=NW)
        bonus_hearts.append(bonus_heart)
        c.after(20000, add_bonus_heart)

    def pickup_bonus_heart():
        global hp
        shipx, shipy = c.coords(ship)
        for bonus_heart in bonus_hearts:
            bonus_heartx, bonus_hearty = c.coords(bonus_heart)
            if (bonus_hearty+60>shipy) and (bonus_hearty<shipy+60) and (bonus_heartx+60>shipx) and (bonus_heartx<shipx+60):
                c.delete(bonus_heart)
                hp+=1
                c.delete(HPLabel)
                label_hp()
                bonus_hearts.remove(bonus_heart)
        c.after(50, pickup_bonus_heart)

    def remove_bullets():
        for bullet in bullets:
            c.move(bullet, 0, -10)
            bulletx, bullety = c.coords(bullet)
            if bullety < -30:
                c.delete(bullet)
                bullets.remove(bullet)
        c.after(5, remove_bullets)

    def add_meteor():
        meteor = c.create_image(random.randint(0, 410), -80, image=meteor_image, anchor=NW)
        meteors.append(meteor)
        c.after(random.randint(750, 1500), add_meteor)

    def kollision_ship():
        global hp
        for meteor in meteors[:]:
            meteorx, meteory = c.coords(meteor)
            shipx, shipy = c.coords(ship)
            if (meteory + 80 > shipy) and (meteory < shipy + 60) and (meteorx + 40 > shipx) and (meteorx < shipx + 60):
                c.delete(meteor)
                meteors.remove(meteor)
                hp -= 1
                GameOver()
                c.delete(HPLabel)
                label_hp()
        for meteor in meteors[:]:
            meteorx, meteory = c.coords(meteor)
            if meteory > 700:
                c.delete(meteor)
                meteors.remove(meteor)
        c.after(50, kollision_ship)

    def moving_meteors():
        for meteor in meteors:
            c.move(meteor, 0, 3)
        c.after(50, moving_meteors)

    def GameOver():
        global hp
        global score
        global window_height
        global window_width
        if hp == 0:
            c.delete(ship)
            c.create_text(110, 325, width=500, text="Game over", fill='#FF0000', anchor=NW, font=("Courier", 44))
            c.after(3000, lambda: exit())

    def kollision_bonus_heart():
        for i, bonus_heart in enumerate(bonus_hearts):
            for j, meteor in enumerate(meteors):
                bonus_heartx, bonus_hearty = c.coords(bonus_heart)
                meteorx, meteory = c.coords(meteor)
                if (meteory + 80 > bonus_hearty) and (meteory < bonus_hearty + 60) and (meteorx + 40 > bonus_heartx) and (meteorx < bonus_heartx + 60):
                    c.delete(bonus_heart)
                    bonus_hearts.pop(i)
                    break
        c.after(50, kollision_bonus_heart)

    def ShootingBonusHearts():
        for i, bullet in enumerate(bullets):
            for j, bonus_heart in enumerate(bonus_hearts):
                bulletx, bullety = c.coords(bullet)
                bonus_heartx, bonus_hearty = c.coords(bonus_heart)
                if (bonus_hearty + 60 > bullety) and (bonus_hearty < bullety + 30) and (bonus_heartx + 60 > bulletx) and (bonus_heartx < bulletx + 5):
                    c.delete(bullet)
                    c.delete(bonus_heart)
                    bullets.pop(i)
                    bonus_hearts.pop(j)
                    break
        c.after(50, ShootingBonusHearts)

    def ShootingMeteors():
        global score
        for i, bullet in enumerate(bullets):
            for j, meteor in enumerate(meteors):
                bulletx, bullety = c.coords(bullet)
                meteorx, meteory = c.coords(meteor)
                if (meteory + 80 > bullety) and (meteory < bullety + 30) and (meteorx + 40 > bulletx) and (meteorx < bulletx + 5):
                    c.delete(bullet)
                    c.delete(meteor)
                    meteors.pop(j)
                    bullets.pop(i)
                    score+=10
                    c.delete(SCORELabel)
                    label_score()
                    break
        c.after(50, ShootingMeteors)

    tk.bind("<KeyPress>", knopki)
    add_meteor()
    moving_meteors()
    remove_bullets()
    kollision_ship()
    kollision_bonus_heart()
    label_hp()
    label_score()
    label_ea()
    score_plus()
    ShootingMeteors()
    add_bonus_heart()
    pickup_bonus_heart()
    ShootingBonusHearts()
def SummonStarter(key):
    if key.char == ' ':
        starter()
tk.bind("<KeyPress>", SummonStarter)
mainloop()
