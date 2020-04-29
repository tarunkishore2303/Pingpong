This consists of a simple python game created using Turtle graphics module.

import turtle
import winsound
import os # For screen clear command




wn = turtle.Screen()
wn.title("Mini project")
wn.bgcolor("black")
wn.setup(width = 800, height = 600)
wn.tracer(0)




#Score
score_a = 0
score_b = 0


#Ball
ball = turtle.Turtle()
ball.speed(0)
ball.shape("circle")
ball.color("white")
ball.penup()
ball.goto(0, 0)
ball.dx = 0.2
ball.dy = -0.2

# Pen
pen = turtle.Turtle()
pen.speed(0)
pen.color("white")
pen.penup()
pen.hideturtle()
pen.goto(0, 260)
pen.write("Player A: 0  Player B: 0",align="center",font=("Courier",24 ,"normal"))

#Mypen
mypen = turtle.Turtle()
mypen.speed(0)
mypen.color("white")
mypen.penup()
mypen.hideturtle()
mypen.goto(0, -260)
mypen.sec = 5

#Winning score
mypen1 = turtle.Turtle()
mypen1.speed(0)
mypen1.color("white")
mypen1.penup()
mypen1.hideturtle()
mypen1.goto(0, 230)
mypen1.sec = 5
mypen1.write("Winning score : 5",align="center",font=("Courier", 20 ,"normal"))


class paddle(turtle.Turtle):
    def properties(self,name,sp,sh,co,x,y):
        self.name = name
        self.speed(sp)
        self.shape(sh)
        self.color(co)
        self.shapesize(stretch_wid=5, stretch_len=1)
        self.penup()
        self.goto(x,y)

a = paddle()
b = paddle()

# "paddle_a", 0, "square", "white", -350, 0
# "paddle_b", 0, "square", "white", 0, -350
a.properties("paddle_a", 0, "square", "white", -350, 0)
b.properties("paddle_b", 0, "square", "white", 350, 0)

#Function to move paddles
def paddle_a_up():
    y = a.ycor()
    y += 20
    a.sety(y)

def paddle_a_down():
    y = a.ycor()
    y -= 20
    a.sety(y)

def paddle_b_up():
    y = b.ycor()
    y += 20
    b.sety(y)

def paddle_b_down():
    y = b.ycor()
    y -= 20
    b.sety(y)

def countdown():
    wn.ontimer(quit,1000)



#Keybindings
wn.listen()
wn.onkeypress(paddle_a_up, "w")
wn.onkeypress(paddle_a_down, "s")
wn.onkeypress(paddle_b_up, "Up")
wn.onkeypress(paddle_b_down, "Down")


#Main game loop
while True:
    wn.update()

    #Move the ball
    ball.setx(ball.xcor() + ball.dx)
    ball.sety(ball.ycor() + ball.dy)

    # Border Check or limitation
    if ball.ycor() > 290:
        ball.sety(290)
        ball.dy *= -1
        winsound.PlaySound('bounce.wav', winsound.SND_ASYNC)  

    if ball.ycor() < -290:
        ball.sety(-290)
        ball.dy *= -1
        winsound.PlaySound('bounce.wav', winsound.SND_ASYNC)  
    if ball.xcor() > 390:
        ball.goto(0, 0)
        ball.dx *= -1
        score_a +=1
        pen.clear()
        pen.write("Player A: {}  Player B: {}".format(score_a,score_b),align="center",font=("Courier",24 ,"normal"))
    if ball.xcor() < -390:
        ball.goto(0, 0)
        ball.dx *= -1
        score_b +=1
        pen.clear()
        pen.write("Player A: {}  Player B: {}".format(score_a,score_b),align="center",font=("Courier",24 ,"normal"))

        

    # Paddle and ball collisions
    if (ball.xcor() > 340 and ball.xcor() < 350) and (ball.ycor() < b.ycor() + 40 and ball.ycor() > b.ycor() -40):
        ball.setx(340)
        ball.dx *=-1
        winsound.PlaySound('bounce.wav', winsound.SND_ASYNC)  
    
    if (ball.xcor() < -340 and ball.xcor() > -350) and (ball.ycor() < a.ycor() + 40 and ball.ycor() > a.ycor() -40):
        ball.setx(-340)
        ball.dx *=-1
        winsound.PlaySound('bounce.wav', winsound.SND_ASYNC)

   

    if score_a == 5 :
        mypen.write("Game over, Player A wins the game",align="center",font=("Courier",24 ,"normal"))
        winsound.PlaySound('gameover.wav', winsound.SND_ASYNC)
        turtle.done()
    elif score_b == 5 :
        mypen.write("Game over Player B wins the game",align="center",font=("Courier",24 ,"normal"))
        winsound.PlaySound('gameover.wav', winsound.SND_ASYNC)
        turtle.done()
