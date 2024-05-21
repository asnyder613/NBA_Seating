# NBA_Seating
Group capstone project for the NBA Future Analytics Stars internship. 

## Project Focus: Business Analytics (A)
Pricing NBA Arena Seats Data Sources: Secondary Market Ticket Sales
Focus areas: Supervised ML, Ticket Pricing, Presentation and Communication

- Each NBA team has to price thousands of single game tickets for 41 home games and then update the price in real time to align with the secondary market.
- Given anonymized historical ticket prices, matchup info and seat location calculate a seat quality score.
- Use the seat quality score and given market data to assign the optimal ticket price to unsold inventory.

## Data
The NBA provided us with a data set containing the seat location, game dates, and historical ticket pricing for the 2022-2023 season at the Chase Center. We also consulted Basketball Reference and collected the correlating win percentages for the Golden State Warriors and the visiting team for each game. 

## Executive Summary
Given this challenge, we developed a model to predict ticket prices throughout a season using the data provided and the win percentages from Basketball Reference. 

Our analysis of the data indicates that ticket prices are highly dependent on seat location, the time remaining until the game, and the quality of the teams playing. 

The model that we developed predicted the best ticket price for a given seat based on the data available, but does not account for the larger business goals that may guide an organization’s ticketing strategy, such as maximizing revenue vs. selling out each game. 

## Exploratory Data Analysis

Here is a map of the Chase Arena that we generated from our model predicting ticket pricing. The colors are log-scaled to better differentiate between the average ticket price by seat, but you can see that the outer level has an average ticket price of around $100 per ticket, while the middle level is closer to an average of $250 per ticket, though there is some notable variation in the front rows and towards the center of the arena. The courtside seats, by comparison have an average ticket price of $980. 

![Model](https://github.com/asnyder613/NBA_Seating/blob/7a05bfe8c90655d0425c64dd260b907a0d2f90d5/Ticket_image.png)

To arrive at a model able to predict these ticket prices, we first did some exploratory data analysis to get a feel for the data provided. One of the notable observations was that there was a single ticket priced at $20,000, as seen in the distribution plot here. 

![Model](https://github.com/asnyder613/NBA_Seating/blob/78a38ae590f941982283bdc5579b971520fc558d/Distribution.png)

The question is whether this is a valid ticket price, or whether it is an error in the data collection. Without knowing the answer to this, we chose to remove it from the model calculation, as a single ticket value so far outside of the “normal” range could skew the model results. 

We also looked at the mean ticket listing price by section to see how much ticket listing prices varied in the lead up to an event. Here, it is evident that the ticket prices in most sections stay relatively consistent, while other sections varied significantly in price. 

![Model](https://github.com/asnyder613/NBA_Seating/blob/4913471b92337a9b6a469e484d9ccddaccc1ae5d/Variance1.png)

This next image zooms into that a bit further, looking at just the ten sections with the MOST variance in ticket price. Here it is a bit more obvious that the most pricing variance occurs in the final 45 days leading up to a game… leading us to the assumption that ticket prices generally increase in the lead-up to an event. 

![Model](https://github.com/asnyder613/NBA_Seating/blob/4913471b92337a9b6a469e484d9ccddaccc1ae5d/Variance2.png)

### Conversation with the Orlando Magic's Director of Business Strategy

The final step in the exploration phase of our project was a conversation with the Orlando Magic’s Director of Business Strategy, Ali Anderson. I had heard Ali speak at the MiT-Sloan Sports Analytics conference and reached out to her to see if she had any insight on how the Magic approached ticket pricing. 

Some highlights from that conversation included that TicketMaster has a “Pricemaster” option which uses variables such as sell-through, average ticket price, number of resale listings, and average ticket price to recommend ticket pricing. Some of these data points were present in our data set; others were not—but it indicated that we were on the right track in thinking that it was possible to predict ticket prices based on the data given. 

She also noted that certain areas of their arena tended to sell out last, such as the upper corners, while first rows of a section and aisle seats tended to sell quicker. 

As we had already observed in the data, Ali confirmed that while the ticket sales cycle for the Magic does depend a lot on opponents and timing, but the price of tickets generally always increases in the lead-up to an event.

Ali suggested that with the right data, it could be useful to rank game quality first, then use that outcome to help predict ticket pricing. 

And finally, she made the key observation that the end decision about ticket pricing comes down to a larger decision regarding volume vs. revenue. 

## Methodology
We opted to drop date/time and “duplicate” variables, leaving only relevant variables to use when predicting ticket prices. We removed approximately 1% of the rows due to missing or incomplete data, which explains why the seating map shown earlier is missing a small number of seats. We also applied a z-score normalization to better compare the dataset, so that the model was better able to compare values across the variables. 

For the seat location data, we chose to use the section categories and names provided in the data, though with further development, it might be possible to create a more detailed seat grouping scheme which accounts for consumer preferences such as aisle seats, first rows in a section, etc. 

From there, we experimented with several models to determine the best means of predicting the ticket listing price, and eventually determined that a Random Forest Regression model was the most accurate and comprehensive for our purpose. 

### Additional Data Scraping

We scraped win percentage data for the Golden State Warriors and each visiting team for each game from Basketball Reference.com, based on the assumption that the quality of each team impacts the ticket price. 

We used Win/Loss Percentage as a general metric for team quality and scraped data for the Warriors and their opponents for every game played, then merged the data by date.

### Feature Selection

We dropped the date/time variables, as well as any duplicate variables, while adding win/loss percentage variables for each team and game. 

![Model](https://github.com/asnyder613/NBA_Seating/blob/20280a045392e7d58e6ebb62a75c79d65fa185a5/variables.png)

## Model Performance and Results

The section name, level, and group variables have the greatest impact on ticket price. 
