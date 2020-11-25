# Snake-Game-using-pygame

import turtle
import time
import random

delay = 0.1
segments = []
score = 0
high_score = 0

#Setting up the screen
wn = turtle.Screen()
wn.title("Snake Game")
wn.bgcolor("green")
wn.setup(width = 600, height = 600)
wn.tracer(0) # turns off the screen updates.

#Snake Head
head = turtle.Turtle()
head.speed(0)
head.shape("square")
head.color("black")
head.penup() # usually the snake will draw on screen as it traverses so we use penup to avoid it.
head.goto(0,0)
head.direction = "stop"

food = turtle.Turtle()
food.speed(0)
food.shape("triangle")
food.color("red")
food.penup()
food.goto(0,100)

#pen
pen = turtle.Turtle()
pen.speed(0)
pen.shape("square")
pen.color("white")
pen.penup()
pen.hideturtle()
pen.goto(0, 260)
pen.write("Score: 0 High Score: 0",align = "center", font = ("Courier", 24, "normal"))


def go_up():
    if head.direction != "down":
        head.direction ="up"
def go_down():
    if head.direction != "up":
        head.direction ="down"
def go_left():
    if head.direction != "right":
        head.direction ="left"
def go_right():
    if head.direction != "left":
        head.direction ="right"


# Functions
def move():
        if head.direction == "up":
            y = head.ycor()
            head.sety(y + 20)

        if head.direction == "down":
            y = head.ycor()
            head.sety(y - 20)

        if head.direction == "left":
            x = head.xcor()
            head.setx(x - 20)

        if head.direction == "right":
            x = head.xcor()
            head.setx(x + 20)

#Keyboard instructions for direction movements
wn.listen()
wn.onkeypress(go_up,"w")
wn.onkeypress(go_down,"z")
wn.onkeypress(go_left,"a")
wn.onkeypress(go_right,"s")

#Main game loop
while True:
    wn.update()

    #check for collisions with the border
    if head.xcor() > 740 or head.xcor() < -740 or head.ycor() > 380 or head.ycor() < -370:
        time.sleep(1)
        head.goto(0,0)
        head.direction="stop"

        # hide the segments after the game is new
        for segment in segments:
            segment.goto(1000, 1000)
        #Clear the segments list
        segments.clear()

        #Reset the score
        score = 0
        #Reset the delay- to slow down the delay when the game restarts
        delay = 0.1

        pen.clear()
        pen.write("Score: {}, High Score: {}".format(score, high_score), align="center",
                  font=("Courier", 24, "normal"))

    #check for head collisions with the body
    for segment in segments:
        if segment.distance(head) < 20:
            time.sleep(1)
            head.goto(0,0)
            head.direction="stop"

            # hide the segments after the game is new
            for segment in segments:
                segment.goto(1000, 1000)
            #Clear the segments list
            segments.clear()

            #Reset the score
            score = 0
            #Reset the delay- to slow down the delay when the game restarts
            delay = 0.1

            pen.clear()
            pen.write("Score: {}, High Score: {}".format(score, high_score), align="center",
                      font=("Courier", 24, "normal"))

    if head.distance(food) < 20:
        # move the food to any random spot.
        x = random.randint(-600, 600)
        y = random.randint(-300, 300)
        food.goto(x, y)

        #Add a segment
        new_segment = turtle.Turtle()
        new_segment.speed(0)
        new_segment.shape("square")
        new_segment.color("grey")
        new_segment.penup()
        segments.append(new_segment)

        #Shorten the delay
        delay -= 0.005

        # Increase the score
        score += 10
        if score > high_score:
            high_score = score

        pen.clear()
        pen.write("Score: {}, High Score: {}".format(score, high_score), align="center",
                  font=("Courier", 24, "normal"))

    #move the end segments in reverse order
    for index in range(len(segments)-1, 0, -1):
        x = segments[index - 1].xcor()
        y = segments[index - 1].ycor()
        segments[index].goto(x, y)


    # move the segment 0 to where the head is
    if len(segments) > 0:
        x = head.xcor()
        y = head.ycor()
        segments[0].goto(x, y)

    move()
    time.sleep(delay)

wn.mainloop()
