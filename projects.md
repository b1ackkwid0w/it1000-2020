<!---it1040-2000 markdown project page--->



# AMY A. SNELL

#### _Information Technology student_

---

> ## CURRENT PROJECTS

* **My Rock, Paper, Scissors Game**

    * IT 1040 Programming Class Final Project
    
    * Language: Python
    
```python
#rock paper scissors game
#by Amy Snell 4/18/2020

import random
import sys
from fractions import Fraction


def get_comp_choice():
#this function simply chooses a random integer for computer's choice
    
    comp_choice = random.randint(1,3)
    return comp_choice

def get_player_choice():
#this function gets the player's choice as an integer
    
    print('1. Rock')
    print('2. Paper')
    print('3. Scissors')
    while True:
        try:
            player_select = int(input('\nWhat will it be? '))
        except Exception: #catching invalid user input
            print('\nInvalid: Please enter the number of your choice selection...')
            continue
        else:
            return player_select #returns integer to get_outcome function 3

def get_outcome():
#this function defines the integers from get_comp_choice and get_player_choice
#then determines the outcome of the round
#value is sent to play_new_game or play_loaded_game
    
    computer = get_comp_choice() #gets random integer from get_comp_choice function 1
    #assigns choices to integers
    if computer == 1:
        computer = 'Rock'
    if computer == 2:
        computer = 'Paper'
    if computer == 3:
        computer = 'Scissors'
        
    player_select = get_player_choice() #gets player's choice from get_player_choice function 2
    #assigns choices to integer
    if player_select == 1:
        player_select = 'Rock'
    if player_select == 2:
        player_select = 'Paper'
    if player_select == 3:
        player_select = 'Scissors'

    #gets the outcome of the game
    if computer == player_select:
        outcome = 'tie'
    if player_select == 'Rock':
        if computer == 'Paper':
            outcome = 'lose'
    if player_select == 'Rock':
        if computer == 'Scissors':
            outcome = 'win'
    if player_select == 'Paper':
        if computer == 'Rock':
            outcome = 'win'
    if player_select == 'Paper':
        if computer == 'Scissors':
            outcome = 'lose'
    if player_select == 'Scissors':
        if computer == 'Rock':
            outcome = 'lose'
    if player_select == 'Scissors':
        if computer == 'Paper':
            outcome = 'win'
            
    #displays outcome
    print('\nYou chose',player_select+'. The computer chose',computer+'. You',outcome+'!')
    return outcome #sends outcome to game data in function 4 or 5
    
def play_new_game():
#this function is called when player selects 'start new game'
#game data is stored and written to new data file on exit
    
    player_name = input(str('\nWhat is your name? '))
    print('\nHello,',player_name+". Let's play!")
    game_data = [] #setting game data empty list
    counter = 0 #setting count at zero
    
    while True: #game play loop
        counter += 1
        print('\nRound',counter)
        outcome = get_outcome() #gets the outcome of the game
        game_data.append(outcome) #stores outcomes of the game
        
        while True: #do next menu loop
            try: 
                print('\nWhat would you like to do?')
                print('\n1. Play again')
                print('2. View Game Statistics')
                print('3. Quit')
                do_next = int(input('\nEnter choice: '))
                
                if do_next == 1: #loops back to game play option
                    break
                
                if do_next < 1 or do_next > 3: #catching out of menu number range
                    raise Exception
                
                if do_next == 2: #game statistics option
                    print('\n'+player_name+', here are your game statistics...')
                    win_count = game_data.count('win')
                    print('Wins:',win_count)
                    lose_count = game_data.count('lose')
                    print('Losses:',lose_count)
                    tie_count = game_data.count('tie')
                    print('Ties:',tie_count)
                    while True:
                        try:
                            win_loss_fraction = win_count / lose_count
                            print('Win/Loss Ratio: ',Fraction(win_loss_fraction).limit_denominator())
                            break
                        except ZeroDivisionError: #catching zero losses count
                            print('Win/Loss Ratio: ',win_count,'/',lose_count)
                            break
                    continue #loops back to play menu
                
                if do_next == 3: #exits program option
                    new_file = open(player_name+'.rps','w')
                    for o in game_data:
                        new_file.write(o)
                        new_file.write('\n')
                    new_file.close()
                    print('See you next time,',player_name+'. Thanks for playing!')
                    exit()
            except Exception: #catching invlaid input from user
                print('Invalid. Please enter the number of your selection...')
                continue
                    
    
def play_loaded_game():
#this function is called when player selects 'load game'
#player's file is read and stored in list for display of stats
#player's file is appended with new game data on exit
    
    while True: #retrieves players game file by player's name
        try:
            player_name = input('\nWhat is your name? ')
            player_file = open(player_name+'.rps','r')
            game_data = [] #creates an empty game data list
        except Exception:
            print('\n'+player_name+', your game could not be found.')
            main() #sends back to main menu
            continue
        else:
            try:
                with player_file as f: #reads data and appends to game_data list
                    for data in f:
                        game_data.append(data.strip('\n'))
            except Exception as e: #catching corrupt file data
                print('An error occurred reading game data...')
                print('Error: ',e)
                main() #returns to main menu
            else:
                break
            
    print('\nWelcome back,',player_name,". Let's Play!")
    counter = len(game_data) #counts number of rounds in game_data list
    while True: #play menu loop
        print('\nWhat would you like to do?')
        print('\n1. Play again')
        print('2. View Game Statistics')
        print('3. Quit')
        try:
            do_next = int(input('\nEnter choice: '))
            
            if do_next == 1: #game play loop option
                counter += 1
                print('\nRound',counter)
                outcome = get_outcome()
                game_data.append(outcome)
                continue
            
            if do_next < 1 or do_next > 3: #catching numbers out of menu range
                raise Exception
            
            if do_next == 2: #game statistics option
                print('\n'+player_name+', here are your game statistics...')
                win_count = game_data.count('win')
                print('Wins:',win_count)
                lose_count = game_data.count('lose')
                print('Losses:',lose_count)
                tie_count = game_data.count('tie')
                print('Ties:',tie_count)
                while True:
                        try:
                            win_loss_fraction = win_count / lose_count
                            print('Win/Loss Ratio: ',Fraction(win_loss_fraction).limit_denominator())
                            break
                        except ZeroDivisionError: #catching zero lose count
                            print('Win/Loss Ratio: ',win_count,'/',lose_count)
                            break
                continue #loops back to play menu
            
            if do_next == 3: #exit game option
                new_file = open(player_name+'.rps','a') #writes game_data list to each line of file
                for o in game_data:
                    new_file.write(o)
                    new_file.write('\n')
                new_file.close() #close the file
                print('See you next time,',player_name,'. Thanks for playing!')
                exit() #exits the program
                
        except Exception: #catching invalid input from user
            print('\nInvalid. Please enter the number of your selection...')
            continue #loops back to play menu


#defining main
def main():
    print('\nWelcome to Rock, Paper, Scissors!') #main menu
    print('\n1. Start New Game')
    print('2. Load Game')
    print('3. Quit')
    while True:
        try:
            choice = int(input('\nEnter choice: '))
            if choice < 1 or choice > 3: #catching numbers out of range for menu
                raise Exception
        except Exception: #catching invalid input from user
            print('\nInvalid. Please enter the number of your selection...')
            continue
        else:
            break
    if choice == 1:
        play_new_game() #goes to play_new_game function 4
    if choice == 2:
        play_loaded_game() #goes to play_loaded_game function 5
        
    if choice == 3: #exits the program
        try:
            print('\nSee you next time,',player_name,'. Thanks for playing!')
        except Exception: #catching no player name found
            print('Awww, Leaving so soon? See you next time...')
        finally:
            exit()

main()```

---


* **My Recursive Turtle Program**

    * IT 1040 Programming - Recursion Challenge
    
    * Language: Python
    
```python
##this program uses recursion to create turtle graphic images
#by Amy Snell 04/22/2020

import turtle as t
import random

def optical_lines_1(x1,y2):
    y1 = 370
    x2 = -710
    if x1 >= -710:
        t.penup()
        pen_color = pen_colors()
        t.pencolor(pen_color)
        t.goto(x1,y1)
        t.pendown()
        t.goto(x2,y2)
        t.penup()
        optical_lines_1(x1 - 10,y2 - 10)
        
def optical_lines_2(x4,y3):
    t.clone()
    x3 = 710
    y4 = -370
    if y3 > -370:
        t.penup()
        pen_color = pen_colors()
        t.pencolor(pen_color)
        t.goto(x3,y3)
        t.pendown()
        t.goto(x4,y4)
        t.penup()
        optical_lines_2(x4 - 10,y3 - 10)
        
def optical_lines_3(x6,y5,x8,y7,x9,y10,x11,y12):
    x5 = 0
    y6 = 0
    x7 = 0
    y8 = 0
    y9 = 0
    x10 = 0
    y11 = 0
    x12 = 0
    if x6 < 370:
        t.penup()
        pen_color = pen_colors()
        t.pencolor(pen_color)
        t.goto(x5,y5)
        t.pendown()
        t.goto(x6,y6)
        t.penup()
        u = t.clone()
        u.penup()
        pen_color = pen_colors()
        u.pencolor(pen_color)
        u.goto(x7,y7)
        u.pendown()
        u.goto(x8,y8)
        u.penup()
        r = t.clone()
        r.penup()
        pen_color = pen_colors()
        r.pencolor(pen_color)
        r.goto(x9,y9)
        r.pendown()
        r.goto(x10,y10)
        r.penup()
        pen_color = pen_colors()
        l = t.clone()
        l.penup()
        l.pencolor(pen_color)
        l.goto(x11,y11)
        l.pendown()
        l.goto(x12,y12)
        l.penup()
        star_stamps()
        optical_lines_3(x6 + 5,y5 - 5,x8 - 5,y7 - 5,x9 + 5,y10 + 5,x11 - 5,y12 + 5)
                    
def pen_colors():
    color_num = random.randint(1,7)
    if color_num == 1:
        color_num = 'white'
    if color_num == 2:
        color_num = 'red'
    if color_num == 3:
        color_num = 'blue'
    if color_num == 4:
        color_num = 'yellow'
    if color_num == 5:
        color_num = 'purple'
    if color_num == 6:
        color_num = 'green'
    if color_num == 7:
        color_num = 'orange'
    return color_num

def star_stamps():
    for i in range(1):
        x_goto = random.randint(-700,700)
        y_goto = random.randint(-400,400)
        t.goto(x_goto,y_goto)
        pen_color = pen_colors()
        t.pencolor(pen_color)
        t.dot()
        t.penup()
    
        
def main():
    t.setup(1850,1200)
    t.bgcolor('black')
    t.title('Recursive Turtle Program')
    t.hideturtle()
    t.speed('fastest')
    optical_lines_1(0,370)
    optical_lines_2(710,370)
    optical_lines_3(0,370,0,370,0,-370,0,-370)
    t.penup()
    t.goto(-350,-350)
    t.pendown()
    t.pencolor('white')
    t.write('Thanks for viewing...',font=('Arial',16,'italic'))


main()
```


---

[HOME](README.md)
