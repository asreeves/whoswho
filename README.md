# whoswho
Replication materials for 'The decline and persistence of the Old Boy: Private Schools and Elite Recruitment 1897-2016'.

The raw data used in this analysis are proprietary and so cannot be released. Instead, we share the code used to conduct the analysis so that it can be inspected. 

## Data structure

Each row is a person in Who's Who. Columns are variables, such as the school or university attended by the person included. Most of the analysis is conduct by birth cohort. For example, we are interested in the proportion of people in Who's Who that attended a Clarendon school and who were born between 1830 and 1834. The analysis has been conducted in STATA v.14 and in R. 

## Main analysis

First we examine the time trends for each educational category using an OLS regression model and time dummies.


```
qui reg public i.birth5 if birth5>1 & birth5<30
margins birth5, post

qui reg clarendon_v2 i.birth5 if birth5>1 & birth5<30
margins birth5, post
	
qui reg independent_v2 i.birth5 if birth5>1 & birth5<30
margins birth5, post
		
qui reg other_senior i.birth5 if birth5>1 & birth5<30
margins birth5, post
```


We export the data, multiply the point estimates by 100 (to push them onto percentage point scale), and redraw the graph as a simple line plot. 

```
twoway (line clarendon100 time) (scatter clarendon100 time, mcolor(black)) ///
	(line public100 time, lcolor(green)) (scatter public100 time, mcolor(green)) ///
	(line independent100 time, lcolor(cranberry)) ///
  	(scatter independent100 time, mcolor(cranberry) msymbol(O)) ///
	(line other100 time) (scatter other100 time, mcolor(sea) ) , xlabel(1(1)28, value angle(45)) ///
	ylabel(0(10)75) legend(off) text(67 14 "All Private" "Schools", color(cranberry))  ///
	text(42 14 "All HMC Schools", color(green)) ///
	text(10 14 "Clarendon Schools", color(black)) ///
	text(29 14 "Other senior" "schools", color(sea)) xtitle(Year of birth) ///
	ytitle("Percentage of people in Who's Who" "that attended a particular school") 
```



Figure 1: Who’s Who entrants by school type, 1830-1969 cohorts

![Figure1](./schools_over_time_fig1.png)


Next, we conduct a similar analysis but now we look only at the year in which people were added to Who's Who rather than the year in which they were born.


```
reg public i.first5  if  birth5>1 & birth5<30
margins first5, post

reg clarendon_v2 i.first5  if birth5>1 & birth5<30
margins first5, post

reg independent_v2 i.first5 if  birth5>1 & birth5<30
margins first5, post
```

Again, we export the point estimates, multiply by 100, and plot. 

```
twoway (line clarendon100 time) (scatter clarendon100 time, mcolor(black)) ///
	(line public100 time, lcolor(green)) (scatter public100 time, mcolor(green)) ///
	(line independent100 time, lcolor(cranberry)) ///
	(scatter independent100 time, mcolor(cranberry) msymbol(O)) ///
	if time>3, xlabel(4(1)6, value ) xscale(range(3.75 6.25)) yscale(range(0 55)) ///
	ylabel(0(5)50) legend(off) text(67 14 "All Independent" "Schools", color(cranberry))  ///
	text(33 5 "All HMC Schools", color(green)) ///
	text(10 5 "Clarendon Schools", color(black)) ///
	text(47 5 "All Private schools", color(cranberry)) xtitle(Year added to Who's Who) ///
	ytitle("Proportion of people added to Who's Who" "that attended a particular school") 

```

Figure 4: The persistence of old boys in Who’s Who, 2001-2016 


![Figure2](./schools_recent_fig2.png)
