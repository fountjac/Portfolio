# Final Grade Reflection

## What Grade Did You Earn? State it clearly in one sentence. You can include plus/minus modifiers; see below about those.
I'm giving myself a solid B; I met all of the objectives, but I don't think I exceeded any expectations.

## How Did You Demonstrate That Grade? 
### Import, manage, and clean data:
My main dataset is large and has a lot of variables, so I will be making subsets as I work through the analysis.
Some of the data is already split into different sets, so I will be pulling those bits of information together as well.
Throughout this project I have had to manipulate the data several times to get it into a workable format.

Importing: 
````
allchars <-read_tsv(here::here("data-raw","dnd_chars_all.tsv"))
````
Managing: I had to subset, manipulate, and pivot the data to get it in the format I wanted
````
allchars %>%
	subset(level<2)%>%
	#Manipulate Data: Calculating Means
 summarise(STR=mean(Str),DEX=mean(Dex),
		   CON=mean(Con),INT=mean(Int),
		   WIS=mean(Wis),CHA=mean(Cha)) %>%
	#Pivot Table and Ordering Abilities
 pivot_longer(
    cols = `STR`:`CHA`,
    names_to = "Stat",
    values_to = "AverageValue")%>%
	mutate(Ability=fct_relevel(Stat,"STR","DEX","CON","INT","WIS","CHA"))%>%
````

### Create graphical displays and numerical summaries of data for exploratory analysis and presentation:
Graphical Display; Bar Chart for level range distribution: 
```{r Distribution of Adventurer Levels}
allchars %>%
    #Ready Data
  count(levelGroup) %>%
  mutate(levelG=fct_relevel(levelGroup,"1-3","4-7","8-11","12-15",
  	"16-18","19-20"))%>%
	#Build Bar Chart
  ggplot(aes(x=levelG,y=n,color=levelG))+geom_bar(stat="identity")+
    geom_text(aes(label=n), vjust=-.3, color="black", size=3.5)+
    theme(legend.position="none")+
	labs(title="Distribution of Adventurers by Level Range",
	x="Level Range",y="Count")
```
![levelBarChart](https://github.com/fountjac/Portfolio/blob/main/LevelBarChart.png)

Exploratory Analysis; Five Number Summary for each ability score:
```{r Five Number Summaries of Stats}
allchars %>%
	subset(level<2)%>% #for some reason =1 wasn't properly subsetting
	summarise(Variable=c("Min","Q1","Median","Mean","Q3","Max")
			  ,STR=summary(Str),DEX=summary(Dex),
		   CON=summary(Con),INT=summary(Int),
		   WIS=summary(Wis),CHA=summary(Cha))
```
![5numSum](https://github.com/fountjac/Portfolio/blob/main/5numSum.png)

### Write R programs for simulations from probability models and randomization-based experiments:
To demonstrate using probability models I've worked on Activity 11:
```{r generate n samples}
generate_n_samples<-function(n,mean,sd,r,df,min,max){
  tibble(
    norm_values=rnorm(n=n,mean=mean, sd=sd),
    exp_values = rexp(n=n, r=r),
    chisq_values=rchisq(n=n,df=df),
    uniform_values=runif(n=n,min=min,max=max)
  )
}
generate_n_samples(1,5,.05,4,2,1,11)
```

To tie this objective in with my project I created code to output 10 random level 1 characters from the dataset
```{r random level 1 character stats}
set.seed(12345)                          # Set seed for reproducibility

allchars %>%
	subset(level<2, processedRace != "NA")%>%
	sample_n(10)%>%
	select(processedRace, class, Str:Cha)
```
![RandomChars](https://github.com/fountjac/Portfolio/blob/main/RandomChars.png)


### Use source documentation and other resources to troubleshoot and extend R programs:
Throughout this project Google has been my best friend. We've had a long friendship through other coding classes and I've applied the troubleshooting skills I've learned from coding in SAS to coding in R. I usually find my solution by searching the problem in Google, reading through examples of other people's code and figuring out how I need to change my code to accomplish what their code is demonstrating. 

To extend the R program I've loaded tidyverse.

### Write clear, efficient, and well-documented R programs:
The other examples throughout the portfolio should demonstrate this. My code has short and neccessary comments aiming to ensure others can tell what my code is accomplishing.

