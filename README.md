# fitTrkr
A Fitness and Workout Tracking app called fitTrkr using [Flask](https://github.com/pallets/flask) Backend with MySQL from Scratch. 

### Requirements
* Python
* pip
* MySQL

### Setting up the Project
1. Use pip to install required python libraries.
`
pip install flask
pip install flask_mysqldb
pip install wtforms
pip install passlib
pip install functools
`
2. Setup the database with MySQL
`CREATE DATABASE fitTrkr;`

### Run the Project
`cd FitnessTracker`

`python app.py`

`Point yor browser to [Localhost](https://localhost:5000)`

#### USE myflaskapp db using MySQL
`USE fitTrkr;`

```
CREATE TABLE MealPlan
	(
	MealPlanID    	INT(11) AUTO_INCREMENT,
	MealPlanName 	VARCHAR(30) UNIQUE,
	Category    	VARCHAR(50),
	DietaryRestrictions	 VARCHAR(200),
	MealPlanDescription	 VARCHAR(500),
	Primary Key(MealPlanID)
	);
```

```
CREATE TABLE Meals
	(
	MealID INT(11) AUTO_INCREMENT,
	MealType	VARCHAR(30),
	MealName	VARCHAR(50) UNIQUE,
	CaloriesPerServing	INT,
	DietaryRestrictions	VARCHAR(300),
	MealDescription    	VARCHAR(500),
	Primary Key(MealID)
	);
```

```
CREATE TABLE WorkoutPlan
	(
	WorkoutPlanID INT(11) AUTO_INCREMENT,
	WorkoutPlanName VARCHAR(30) UNIQUE,
	Intensity    	VARCHAR(30),
	PlanDescription	VARCHAR(500),
	Primary Key (WorkoutPlanID)
	);
```

```
CREATE TABLE Workouts
	(
	WorkoutID INT(11) AUTO_INCREMENT,
	Intensity VARCHAR(30),
	WorkoutDescription VARCHAR(500),
	Equipment VARCHAR(300),
	WorkoutName VARCHAR(140) UNIQUE,
	Primary Key (WorkoutID)
	);
```

```
CREATE TABLE Cardio
	(
	WorkoutID INT(11) AUTO_INCREMENT,
	Distance VARCHAR(30),
	Duration VARCHAR(30),
	CardioType VARCHAR(30),
	Primary Key (WorkoutID),
	Foreign Key(WorkoutID) references Workouts(WorkoutID)
	ON DELETE CASCADE
	ON UPDATE CASCADE
	);
```

```
CREATE TABLE Strength
	(
	WorkoutID INT(11) AUTO_INCREMENT,
	BodyPart VARCHAR(50),
	StrengthType VARCHAR(30),
	Primary Key (WorkoutID),
	Foreign Key (WorkoutID) references Workouts(WorkoutID)
	    ON DELETE CASCADE
	    ON UPDATE CASCADE
	);
```

```
CREATE TABLE MealPlan_Meal
	(
	MealPlanID INT(11) AUTO_INCREMENT,
	MealPlanName VARCHAR(50),
	MealID INT(11),
	MealTime CHAR(50),
	Primary Key(MealplanID, MealID),
	Foreign Key(MealplanID) references MealPlan(MealplanID)
		ON DELETE NO ACTION
		ON UPDATE CASCADE,
	Foreign Key(MealID) references Meals(MealID)
		ON DELETE NO ACTION	
		ON UPDATE CASCADE
	);
```

```
CREATE TABLE Workout_Comprise_WPlan
	(
	WorkoutPlanID INT(11),
	WorkoutPlanName VARCHAR(50) ,
	WorkOutID INT(11),
	NumSets INT(11),
	NumReps INT(11),
	Primary Key(WorkoutPlanID, WorkOutID),
	Foreign Key(WorkoutPlanID) references WorkoutPlan(WorkoutPlanID)
		ON DELETE NO ACTION
        ON UPDATE CASCADE,
	Foreign Key(WorkOutID) references Workouts(WorkOutID)
		ON DELETE NO ACTION
		ON UPDATE CASCADE
	);
```

```
CREATE TABLE PostalCode
(
	PostalCode VARCHAR(6),
	City VARCHAR(100),
	ProvinceState VARCHAR(30),
	Country VARCHAR(100),
	Primary Key(PostalCode)
);
```

```
CREATE TABLE Users
(
	UserID INT(11) AUTO_INCREMENT,
	UserName VARCHAR(30) UNIQUE,
	FirstName VARCHAR(30),
	LastName VARCHAR(30),
	PasswordHash CHAR(64),
	PasswordSalt CHAR(32),
	Gender VARCHAR(10),
	Age INT(11),
	Address VARCHAR(100),
	PostalCode VARCHAR(6),
	Primary Key (UserID),
	Foreign Key(PostalCode) references PostalCode(PostalCode)
		ON DELETE NO ACTION 
		ON UPDATE CASCADE
	);
```

```
CREATE TABLE Session
(
	UserID INT(11) AUTO_INCREMENT,
	Token CHAR(128),
	Primary Key (UserID, Token),
	Foreign Key(UserID) references Users(UserID)
		ON DELETE CASCADE
		ON UPDATE CASCADE
	);
```


```
CREATE TABLE Trainers
    (
	UserID INT(11),
	TrainerFocus VARCHAR(300),
	Primary Key (UserID),
	Foreign Key(UserID) references Users(UserID)
		ON DELETE CASCADE
		ON UPDATE CASCADE
	);
```

```
CREATE TABLE FitnessProgram
	(
	FitnessProgramID INT(11) AUTO_INCREMENT,
	FitnessProgramName VARCHAR(30) UNIQUE,
	FP_intensity VARCHAR(50),
	Description VARCHAR(400),
	Program_Length VARCHAR(20),
	TrainerID INT(11),
	WorkoutPlanID INT NOT NULL,
	MealPlanID INT NOT NULL,
	Primary Key (FitnessProgramID),
	Foreign Key (TrainerID) references Users(UserID)
		ON DELETE SET NULL
		ON UPDATE CASCADE,
	Foreign Key(WorkoutPlanID) references WorkoutPlan(WorkoutPlanID)
		ON DELETE NO ACTION
		ON UPDATE CASCADE,
	Foreign Key(MealPlanID) references MealPlan(MealPlanID)
		ON DELETE NO ACTION
        ON UPDATE CASCADE
	);
```
```
CREATE TABLE Clients
(
	UserID INT(11),
	Height DOUBLE,
	Weight DOUBLE,
	PrimaryGoals VARCHAR(300),
	FitnessLevel VARCHAR(140),
	Current_FitnessProgram INT(11),
	Primary Key(UserID),
	Foreign Key(UserID) references Users(UserID)
		ON DELETE CASCADE
		ON UPDATE CASCADE,
	Foreign Key(Current_FitnessProgram) references FitnessProgram(FitnessProgramID)
	    ON DELETE CASCADE
	    ON UPDATE CASCADE
	);
```
```
CREATE TABLE Logs
	(
	UserID INT(11) NOT NULL,
	LogID INT(11) AUTO_INCREMENT,
	FitnessProgramID INT NOT NULL,
	LogDate DATE,
	Weight DOUBLE,
	WorkoutCompletion INT(11),
	Notes VARCHAR(1000),
	SatisfactionLevel INT(11),
	MealCompletion INT(11),
	Primary Key(LogID),
	Foreign Key(UserID) references Users(UserID)
		ON DELETE CASCADE
		ON UPDATE CASCADE,
	Foreign Key(FitnessProgramID) references FitnessProgram(FitnessProgramID)
		ON DELETE NO ACTION
		ON UPDATE NO ACTION
	);
```

```
CREATE TABLE Preq_Fitness
	(
	PrereqProgramID INT(11) AUTO_Increment,
	FitnessProgramID INT(11),
	Primary key (PrereqProgramID, FitnessProgramID),
	Foreign key(PrereqProgramID) references FitnessProgram(FitnessProgramID)
		ON DELETE NO ACTION
		ON UPDATE NO ACTION,
	Foreign key(FitnessProgramID) references FitnessProgram(FitnessProgramID)
		ON DELETE NO ACTION
		ON UPDATE NO ACTION
	);
```
Populating database with sample dataset from local gym:
```
INSERT INTO PostalCode
VALUES
    ('V2A1C8','Penticton','British Columbia','Canada'),
    ('90210','Beverly Hills','California','United States'),
    ('V3K1C8','Coquitlam','British Columbia','Canada'),
    ('12345','Schenectady','New York','United States'),
    ('C1E1H0','Charlottetown','Prince Edward Island','Canada'),
    ('V5A1S6','Burnaby','British Columbia','Canada'),
    ('V3T0A3','Surrey','British Columbia','Canada'),
    ('94304','Palo Alto','California','United States'),
    ('98052','Redmond','Washington','United States');

```
```
INSERT INTO Users(username, firstname, lastname, passwordhash, gender, age, address, postalcode)
VALUES
       ('dp','David','Pham','ae3722f56fde9c87','Male',30,'1330 Schuyler Dr','90210'),
    ('wc','Winfield','Chen','0274cce43ec14814','Male',22,'1600 Pennsylvania Av','V3K1C8'),
    ('gh','Giovanni','HoSang','33afa37d1f677f81','Male',35,'2 Example St','12345'),
    ('jd','Joshua','Deans','5b0ec797486d5091','Female',19,'302 Hastings St','C1E1H0'),
    ('th','Tom','Hanks','aa7e14efdf3a0abf','Male',39,'2 Example St','V2A1C8'),
    ('rv','Richard','Vaughan','34a385e52d3a7216','Male',40,'8888 University Dr','V5A1S6'),
    ('bf','Brian','Fraser','ff3e800da89c26d6','Male',36,'13450 102 Av','V3T0A3'),
    ('em','Elon','Musk','8a8a6160a0e66693','Male',38,'3500 Deer Creek Rd','94304'),
    ('bg','Bill','Gates','323b80f823690d94','Male',42,'15010 NE 36 St','98052'),
    ('bm','Bill','Murray','323b80f823690d94','Male',69,'15010 NE 36 St','98052');

```

```
INSERT INTO Meals
VALUES
    (1, 'Weight Loss', 'Chicken and Rice', 500, ' ', 'Pan fry a chicken breast.\n Cook basmati rice in boiling water'),
    (2, 'Weight Gain/Bulk', 'Steak and Eggs', 1000, 'Ketogenic', 'Pan sear any filet of beef that you’d like. \nPan fry two eggs in butter'),
    (3, 'Weight Loss', 'Beet and Ricotta Salad', 350, 'Vegetarian', 'Peel and Roast Beets.\nWhip ricotta with parsley and mint.\nThrow in arugula.'),
    (4, 'Weight Gain/Bulk', 'Braised Pork Poutine', 890, ' ', 'Slow braise pork.\nDeep fry potatoes cut in desired shape.\nCover Fries in Store Bought Gravy and Cheese Curds.\nAdd slow braised pork'),
    (5, 'Weight Loss', 'Apple Salad', 200, 'Vegetarian, Vegan, Lactose Intolerant', 'Cut Apples.\nPut in bowl.\n' ),
    (6, 'Weight Loss', 'Lamb and Rice', 1500, ' ', 'Pan fry a lamb breast.\n Cook basmati rice in boiling water'),
    (7, 'Weight Gain/Bulk', 'Salmon and Eggs', 800, 'Ketogenic', 'Pan sear any filet of salmon that you’d like. \nPan fry two eggs in butter'),
    (8, 'Weight Loss', 'Beet and Feet Salad', 350, 'Vegetarian', 'Peel and Roast Beets.\nWhip ricotta with parsley and mint.\nThrow in severed feet.'),
    (9, 'Weight Gain/Bulk', 'Braised Beef Heart Poutine', 490, ' ', 'Slow braise beef heart.\nDeep fry potatoes cut in desired shape.\nCover Fries in Store Bought Gravy and Cheese Curds.\nAdd slow braised pork'),
    (10, 'Weight Loss', 'Keep Salad', 200, 'Vegetarian, Vegan, Lactose Intolerant', 'Cut Apples.\nPut in bowl.\n' ),
    (11, 'Weight Loss', 'Johnny Pork and Rice', 700, ' ', 'Pan fry a Johnny breast.\n Cook basmati rice in boiling water'),
    (12, 'Weight Gain/Bulk', 'Trout and Eggs', 800, 'Ketogenic', 'Pan sear any filet of salmon that you’d like. \nPan fry two eggs in butter'),
    (13, 'Weight Loss', 'Beet and Pork Salad', 350, 'Vegetarian', 'Peel and Roast Beets.\nWhip ricotta with parsley and mint.\nThrow in severed feet.'),
    (14, 'Weight Gain/Bulk', 'Braised jonny Poutine', 490, ' ', 'Slow braise beef heart.\nDeep fry potatoes cut in desired shape.\nCover Fries in Store Bought Gravy and Cheese Curds.\nAdd slow braised pork'),
    (15, 'Weight Loss', 'Feet Salad', 700, 'Vegetarian, Vegan, Lactose Intolerant', 'Cut Feet.\nPut in bowl.\n' );
    
```
```
INSERT INTO MealPlan

VALUES
    (1,'Green is good', 'Weightloss','Vegetarian','Easy to prepare for the on the go working professional who has a moral compass'),
    (2,'Gonna get jacked','Weight Gain/Bulk','None','Caloric heavy meal plan to gain weight and develop muscle'),
    (3,'Basic is best','Weightloss','Vegetarian','Simple lte meal plan for vegetarians'),
    (4,'Powered by quinoa','Weight Gain/Bulk','Vegetarian','Meal plan for the bodybuilder who loves chickpeas'),
    (5,'I hate my arteries','Weight Gain/Bulk','None','Quick and dirty weight gain');

```
```
INSERT INTO Workouts
VALUES
    (1, 'HARD', 'Grab a Weight.\nSit down and tuck elbow into thigh.\nCurl weight with bicep', 'Barbell','Bicep Curl'),
    (2, 'HARD', 'Grab Weights.\nSit on bench with back up and flat.\nRaise Shoulders and make two 90 degree angles with arms.\nPush weights into air.\n', 'Barbell', 'Shoulder Press' ),
    (3, 'Easy', 'Lie facedown on floor.\nPut hands, palm first,  shoulder length apart on floor.\nPush up.', 'None', 'Push up'),
    (4, 'Easy', 'Lie on back with feet on floor.\nHands behind back and lift up using abs.', 'None', 'Sit Up'),
    (5, 'Easy', 'Push up on bars.\nDip using triceps', 'supporting ledge of some kind', 'Tricep dip'),
    (6, 'Easy', 'Move legs forward briskly', 'None', 'Jog'),
    (7, 'Easy', 'Get down on all fours.\nWalk by moving forward with hands then feet.', 'None', 'Crawl'),
    (8, 'Easy', 'Jump in air.\nSpread legs and arms like eagle', 'None', 'Jumping Jack'),
    (9, 'Easy', 'Jump in air.\nTuck knees in at peak', 'None', 'Knee Tucks'),
    (10, 'Med', 'Jump in air.\nReach up as med as you can with one hand','None', 'med Jump'),
    (11, 'Easy', 'Move legs forward briskly but not too briskly', 'None', 'Speed walking'),
    (12, 'Med', 'Get down on all three.\nWalk by moving forward with hand then feet.', 'None', 'Baby Crawl'),
    (13, 'Easy', 'Jump in air.\nSpread legs and arms like eagle', 'None', 'Small Jack'),
    (14, 'Med', 'Jump in pool.\nTuck knees in at peak', 'None', 'Canon Ball'),
    (15, 'Easy', 'Jump in air.\nReach up as low as you can with one hand','None', 'small Jump');
    
```
```
INSERT INTO WorkoutPlan
VALUES
    (1,'P 100 Y', 'HIGH', 'For serious athletes looking to get serious'),
    (2,'Tim Tebow Training', 'LOW', 'For non-serious plebians looking to stay non-serious'),
    (3,'Hillary Clinton Module', 'MED', 'Maintenance for the average joe/chloe'),
    (4,'The Express Train to Pain', 'HIGH', 'For people who really enjoy suffering'),
    (5,'Show Muscles Please!', 'LOW', 'For people who only want the illusion of working out');
```
```
INSERT INTO Trainers
VALUES
    (6, 'Getting you fit' ),
    (7, 'Getting you hot'),
    (8, 'Dedicated to bringing out the inner athlete in you'),
    (9, 'Professional Athlete Development'),
    (10, 'Beginners');
```
```
INSERT INTO FitnessProgram
VALUES
    (1,'The Hillary', 'High','To get you hot','13 weeks',6, 1, 1),
    (2,'The Monica','Low','To get you sexy','10 weeks',6, 4, 1),
    (3,'Oprah for President','Medium','Love yourself','4 weeks',7,2,2),
    (4,'The Tony workout itsss great','High','Become a fierce Tiger','10 weeks',8,5,4),
    (5,'Dragon child', 'High','Dragon Penultimate Form','10 weeks',9,3,4),
    (6,'Rainbow 6 for chicks','High','Unicorn Final Form','15 weeks',10,5,5),
    (7,'Killer Mikes Sweat', 'High','Damnnnnn','8 weeks',10, 1, 1),
    (8,'Dani Storms fitness','Low','You too can be famous','9 weeks',10, 4, 1),
    (9,'The Rock for President','Medium','Can you smell what im sweating','3 weeks',10,2,2),
    (10,'The Grunt','High','The guy that no one wants to be at the gym','4 weeks',10,5,4),
    (11,'Unicorn is my spirit animal', 'High','ITS SO FLUFFYYYYYY','6 weeks',10,3,4),
    (12,'No land for wimps','High','Hard as hell','12 weeks',10,5,5);

```
```
INSERT INTO Clients
VALUES
    (1,1.7,68,'Muscle','Low',1),
    (2,1.8,70,'Endurance','High',2),
    (3,1.65,65,'Muscle','Low',3),
    (4,1.75,75,'Muscle','Low',4),
    (5,1.85,85,'Weight loss','Medium',5);

```
```
INSERT INTO MealPlan_Meal
VALUES
    (1,'Green is Clean',3,'lunch/dinner' ),
    (1,'Green is great',5,'breakfast'),
    (2,'Delishhh!',3, 'lunch/dinner'),
    (2,'Delishhh!',1,'lunch/dinner'),
    (2,'Delishhh!!',4,'lunch/dinner'),
    (1,'Green is Clean',7,'lunch/dinner' ),
    (1,'Green is Clean',8,'breakfast/dinner' ),
    (1,'Green is Clean',9,'breakfast/lunch' ),
    (1,'Green is Clean',10,'breakfast/dinner' ),
    (1,'Green is Clean',11,'lunch/dinner' ),
    (1,'Green is Clean',12,'lunch/dinner' ),
    (2,'Delishhh!',6, 'lunch/dinner'),
    (2,'Delishhh!',7,'lunch/dinner'),
    (2,'Delishhh!',5,'lunch/dinner');




```

```
INSERT INTO Preq_Fitness
VALUES
    (1,2),
    (2,3),
    (3,4),
    (4,5),
    (5,6);

```

```
INSERT INTO Cardio
VALUES
    (6, '500 km', 'Until you complete', 'Endurance/Fat Burn'),
    (7, '5 km', '50 mins', 'Endurance/Speed'),
    (8, 'N/A', '2 mins', 'Endurance/Fat Burn'),
    (9, 'N/A', '2 mins', 'Endurance/Speed'),
    (10, 'N/A', '2 mins', 'Strength');

```

```
INSERT INTO Strength
VALUES
    (1, 'Bicep', 'Explosive'),
    (2, 'Shoulders', 'Explosive'),
    (3, 'Chest, Shoulders', 'Endurance'),
    (4, 'Abs', 'Endurance'),
    (5, 'Triceps', 'Endurance');

```
```
INSERT INTO Workout_Comprise_WPlan
VALUES
    (1,'Insane to the Membrane', 2,3,8),
    (1,'Get ready to have a bubble butt',1,3,8),
    (2,'Lets get you in that polka dot dress',1,1,1),
    (2,'Try not to Cry',2,1,1),
    (3,'Sweating out of your eyeballs',1,2,12);

```

```
INSERT INTO Logs(UserId, FitnessProgramID, LogDate, Weight, WorkoutCompletion, Notes, SatisfactionLevel, MealCompletion)
VALUES
    (1,1,'2018-05-01',90.5,5,'feels good',8,9),
    (2,2,'2018-06-01',100.6,7,'feeling hotter',10,10),
    (3,3,'2018-07-01',3000.2,8,'feeling gross', 2,1),
    (4,4,'2018-08-01',120.4 ,10,'So bad, but so good',4,1),
    (5,5,'2018-09-01',250.6,10,'I love my body',10 ,10);

```
Set Up db.py as follows:
```
def DBconfig():
    dbconfig = {
        "host": 'localhost', 
        "user": 'root',
        "password": 'fitness2018!',
        "DBName": 'fitTrkr',
        "dictDB": 'DictCursor',
    }
    return dbconfig
```

