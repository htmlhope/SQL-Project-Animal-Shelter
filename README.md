# SQL-Project-Animal-Shelter
This is the repository containing my Data Analysis Project in SQL. 

I investigated data from the Austin Animal Center to dive deep into the intakes and outakes of the animal shelter. 

Questions I answered:
- What was the Intake Condition of animals that were adopted out?
- What are the most popular coat colors in adopted animals?
- What type of animals were most frequently adopted? 

Visualization for this project was created in Tableau. 

SQL:

SELECT * FROM AustinShelter..Austin_Animal_Center_Intakes$

SELECT [Outcome Type] FROM AustinShelter..Austin_Animal_Center_Outcomes$
WHERE [Outcome Type] IS NOT NULL
GROUP BY [Outcome Type]

SELECT * FROM AustinShelter..Austin_Animal_Center_Outcomes$


--Comparing Intake Condition vs. Outcome Type

CREATE VIEW IntakevsOutcomeRev AS
SELECT COUNT(*) AS 'Counts', intake.[Intake Condition], outc.[Outcome Type]
FROM AustinShelter..Austin_Animal_Center_Intakes$ intake
JOIN AustinShelter..Austin_Animal_Center_Outcomes$ outc
ON intake.[Animal ID]=outc.[Animal ID]
WHERE [Outcome Type] = 'Adoption'
GROUP BY intake.[Intake Condition], outc.[Outcome Type]
ORDER BY 'Counts' DESC

--Total amount of animals by type

SELECT COUNT(*) AS 'Counts', [Animal Type] 
FROM AustinShelter..Austin_Animal_Center_Outcomes$
WHERE [Outcome Type] IS NOT NULL
GROUP BY [Animal Type]
ORDER BY 'Counts' DESC

SELECT [Age upon Outcome], [Animal Type] 
FROM AustinShelter..Austin_Animal_Center_Outcomes$ 
GROUP BY [Age upon Outcome], [Animal Type]
ORDER BY [Age upon Outcome]

--Outcome Type vs. Age upon Outcome.
--The largest age group that was adopted out was 2 months old (0.1666) kittens, the next largest group is the 1 year olds. 
CREATE VIEW AdoptedvsAge AS
SELECT COUNT(*) as 'Count', [Outcome Type], [Age upon Outcome], Breed
FROM AustinShelter..Austin_Animal_Center_Outcomes$
WHERE [Outcome Type] = 'Adoption'
GROUP BY [Age upon Outcome], [Outcome Type], Breed
ORDER BY 'Count' DESC

--The majority of the animals opted out were Domestic Shorthair Mixes (cats)
CREATE VIEW AdoptedBreeds AS
SELECT COUNT(*) as 'Count', Breed
FROM AustinShelter..Austin_Animal_Center_Outcomes$
WHERE [Outcome Type] = 'Adoption'
GROUP BY Breed
ORDER BY 'Count' DESC

--Most popular coat colors (Tabbies were the most popular coat colors)
CREATE VIEW CoatColor AS
SELECT COUNT(*) as 'Count', Breed, Color
FROM AustinShelter..Austin_Animal_Center_Outcomes$
WHERE [Outcome Type] = 'Adoption'
GROUP BY Color, Breed
ORDER BY 'Count' DESC

--Comparing the breed types that were returned to owner
--Pit Bull Mixes make up the majority of the breeds that were returned to owner
CREATE VIEW ReturnedToOwner AS
SELECT COUNT(*) AS 'Count', [Breed]
FROM AustinShelter..Austin_Animal_Center_Outcomes$
WHERE [Outcome Type] LIKE '%Owner%'
GROUP BY Breed
ORDER BY 'Count' DESC
