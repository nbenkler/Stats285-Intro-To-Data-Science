Team HW\#2
================
Noam Benkler, Kitty Miao
Math 285, Winter 2019

``` r
# Should be deleted after the project
# Trial Keys: trial$id == jurors$trial_id == answers$juror_id_trial_id
# Juror Keys: juror$id == answers$juror_id
View(trials) 
View(jurors)
View(answers)
```

Possible topics: 1. In each trial, ratio of black and white jurors 2. Ratio of black and white jurors in each county 3. Race and strike eligibility 4. Among the jurors, geographic information for those who got "struck" 5. TBD...

``` r
#joining table
trial_juror_data <- full_join(x = trials, y = jurors, by = c("id" = "trial__id")) %>% 
  select(id, cause_number, county, prosecutor_1, batson_claim_by_defense, race, gender, struck_by, strike_eligibility)
```

``` r
#simple first plot by race
trial_juror_data %>%
  select(-c(cause_number, batson_claim_by_defense, strike_eligibility))%>%
  mutate(struck_condition = fct_collapse(struck_by,
                               struck = c("Struck by the state",
                                          "Struck for cause", 
                                          "Struck by the defense",
                                          "Struck without notation"),
                               not_struck = c("Juror chosen to serve on jury",
                                              "Juror excused/absent",
                                              "Juror not struck",
                                              "Juror chosen as alternate"))) %>%
  mutate(race = fct_collapse(race,
                             black = "Black",
                             white = "White",
                             other = c("Asian",
                                       "Latino",
                                       "Unknown"))) %>%
  filter(struck_by != "Unknown", race != "other") %>%
  group_by(race) %>%
  mutate(n = n()) %>%
  group_by(race, struck_condition) %>%
  mutate(prop_struck = n()/n) %>%
  group_by(race, struck_condition, prop_struck) %>%
  summarise() %>% 
  ggplot(aes(fct_reorder(race, prop_struck), prop_struck, fill = race)) +
  scale_fill_manual(values=c("black", "white")) +
  facet_wrap(~struck_condition) +
  geom_bar(stat = "identity", color = "black") +
  labs(title = "Proportion of Jurors Struck and not Struck by Race",
       y = "Proportion Struck",
       x = "Race",
       caption = "*Jurors of unknown race not included") +
  theme(
    plot.background = element_rect(fill = "gray93"),
    panel.background = element_rect(fill = "gray80"),
    plot.title = element_text(face = "bold"),
    panel.grid.minor = element_blank(),
    panel.grid.major.x = element_blank(),
    panel.grid.major.y = element_line(linetype = 3, color = "gray50"),
    axis.line.x = element_line(),
    strip.background = element_rect(fill = "gray40"),
    strip.text = element_text(color = "white"),
    legend.position = "none"
  )
```

![](team-hw2-team-hw2-9_files/figure-markdown_github/unnamed-chunk-2-1.png)

``` r
#FILTERED FOR JUST ELIMINATED BY PROSECUTION
trial_juror_data %>%
  select(-c(cause_number, batson_claim_by_defense, strike_eligibility))%>%
  mutate(race = fct_collapse(race,
                             black = "Black",
                             white = "White",
                             other = c("Asian",
                                       "Latino",
                                       "Unknown"))) %>%
  filter(struck_by != "Unknown", race != "other") %>%
  filter(struck_by == "Struck by the state" | struck_by == "Juror chosen to serve on jury") %>%
  group_by(race) %>%
  mutate(n = n()) %>%
  group_by(race, struck_by) %>%
  mutate(prop_struck = n()/n) %>%
  group_by(race, struck_by, prop_struck) %>%
  summarise() %>% 
  ggplot(aes(fct_reorder(race, prop_struck), prop_struck, fill = race)) +
  scale_fill_manual(values=c("black", "white")) +
  facet_wrap(~struck_by) +
  geom_bar(stat = "identity", color = "black") +
  labs(title = "Proportion of Jurors Struck or not Struck by the Prosecution by Race",
       y = "Proportion Struck",
       x = "Race",
       caption = "*Jurors of unknown race not included") +
  theme(
    plot.background = element_rect(fill = "gray93"),
    panel.background = element_rect(fill = "gray80"),
    plot.title = element_text(face = "bold"),
    panel.grid.minor = element_blank(),
    panel.grid.major.x = element_blank(),
    panel.grid.major.y = element_line(linetype = 3, color = "gray50"),
    axis.line.x = element_line(),
    strip.background = element_rect(fill = "gray40"),
    strip.text = element_text(color = "white"),
    legend.position = "none"
  )
```

![](team-hw2-team-hw2-9_files/figure-markdown_github/unnamed-chunk-2-2.png)

``` r
#plots of juror struck by race, by county
trial_juror_data %>%
  select(-c(cause_number, batson_claim_by_defense, strike_eligibility))%>%
  mutate(struck_condition = fct_collapse(struck_by,
                               struck = c("Struck by the state",
                                          "Struck for cause", 
                                          "Struck by the defense",
                                          "Struck without notation"),
                               not_struck = c("Juror chosen to serve on jury",
                                              "Juror excused/absent",
                                              "Juror not struck",
                                              "Juror chosen as alternate"))) %>%
  mutate(race = fct_collapse(race,
                             black = "Black",
                             white = "White",
                             other = c("Asian",
                                       "Latino",
                                       "Unknown"))) %>%
  filter(struck_by != "Unknown", race != "other") %>%
  group_by(race) %>%
  mutate(n = n()) %>%
  group_by(race, county) %>%
  mutate(county_pop = n()) %>%
  group_by(race, county, struck_condition) %>%
  mutate(prop_struck = n()/county_pop) %>%
  group_by(race, struck_condition, prop_struck, county) %>%
  summarise() %>% 
  ggplot(aes(fct_reorder(race, prop_struck), prop_struck, fill = race)) +
  scale_fill_manual(values=c("black", "white")) +
  facet_grid(fct_rev(struck_condition)~county) +
  geom_bar(stat = "identity", position = position_dodge(), color = "black") +
  labs(title = "Proportion of Jurors Struck and not Struck by Race \nWithin Each 5th Circit Court District County",
       y = "Proportion Struck",
       x = "Race",
       caption = "*Jurors of unknown race not included") +
  theme(
    plot.background = element_rect(fill = "gray93"),
    panel.background = element_rect(fill = "gray80"),
    plot.title = element_text(face = "bold"),
    panel.grid.minor = element_blank(),
    panel.grid.major.x = element_blank(),
    panel.grid.major.y = element_line(linetype = 3, color = "gray50"),
    axis.line.x = element_line(),
    strip.background = element_rect(fill = "gray40"),
    strip.text = element_text(color = "white"),
    legend.position = "none"
  )
```

![](team-hw2-team-hw2-9_files/figure-markdown_github/unnamed-chunk-3-1.png)

``` r
#FILTER FOR JUST STATE STIKES
trial_juror_data %>%
  select(-c(cause_number, batson_claim_by_defense, strike_eligibility))%>%
  mutate(race = fct_collapse(race,
                             black = "Black",
                             white = "White",
                             other = c("Asian",
                                       "Latino",
                                       "Unknown"))) %>%
  filter(struck_by != "Unknown", race != "other") %>%
  filter(struck_by == "Struck by the state" | struck_by == "Juror chosen to serve on jury") %>%
  mutate(struck_by = fct_recode(struck_by,
                                "Juror Served"="Juror chosen to serve on jury"))%>%
  group_by(race) %>%
  mutate(n = n()) %>%
  group_by(race, county) %>%
  mutate(county_pop = n()) %>%
  group_by(race, county, struck_by) %>%
  mutate(prop_struck = n()/county_pop) %>%
  group_by(race, struck_by, prop_struck, county) %>%
  summarise() %>% 
  ggplot(aes(fct_reorder(race, prop_struck), prop_struck, fill = race)) +
  scale_fill_manual(values=c("black", "white")) +
  facet_grid(fct_rev(struck_by)~county) +
  geom_bar(stat = "identity", position = position_dodge(), color = "black") +
  labs(title = "Proportion of Jurors Struck and not Struck by the Prosecution \nby Juror Race Within Each 5th Circit Court District County",
       y = "Proportion Struck",
       x = "Race",
       caption = "*Jurors of unknown race not included") +
  theme(
    plot.background = element_rect(fill = "gray93"),
    panel.background = element_rect(fill = "gray80"),
    plot.title = element_text(face = "bold"),
    panel.grid.minor = element_blank(),
    panel.grid.major.x = element_blank(),
    panel.grid.major.y = element_line(linetype = 3, color = "gray50"),
    axis.line.x = element_line(),
    strip.background = element_rect(fill = "gray40"),
    strip.text = element_text(color = "white"),
    legend.position = "none"
  )
```

![](team-hw2-team-hw2-9_files/figure-markdown_github/unnamed-chunk-3-2.png)

``` r
#plot of juror struck by race, by gender
#creating data subset
race_and_gender_all<- trial_juror_data %>%
  select(-c(cause_number, batson_claim_by_defense, strike_eligibility))%>%
  mutate(struck_condition = fct_collapse(struck_by,
                               struck = c("Struck by the state",
                                          "Struck for cause", 
                                          "Struck by the defense",
                                          "Struck without notation"),
                               not_struck = c("Juror chosen to serve on jury",
                                              "Juror excused/absent",
                                              "Juror not struck",
                                              "Juror chosen as alternate"))) %>%
  mutate(race = fct_collapse(race,
                             black = "Black",
                             white = "White",
                             other = c("Asian",
                                       "Latino",
                                       "Unknown"))) %>%
  filter(struck_by != "Unknown", race != "other") %>%
  group_by(race) %>%
  mutate(n = n()) %>%
  group_by(race, gender) %>%
  mutate(gender_comp = n()) %>%
  group_by(race, gender, struck_condition) %>%
  mutate(prop_struck = n()/gender_comp) %>%
  group_by(race, gender, struck_condition, prop_struck) %>%
  summarise() 

#plot
race_and_gender_all%>% 
  ggplot(aes(fct_reorder(race, prop_struck), prop_struck, fill = race)) +
  scale_fill_manual(values=c("black", "white")) +
  facet_grid(fct_rev(struck_condition)~gender) +
  geom_bar(stat = "identity", position = position_dodge(), color = "black") +
  labs(title = "Proportion of Jurors Struck and not Struck by Juror Race and Gender",
       y = "Proportion Struck",
       x = "Race",
       caption = "*Jurors of unknown race not included") +
  theme(
    plot.background = element_rect(fill = "gray93"),
    panel.background = element_rect(fill = "gray80"),
    plot.title = element_text(face = "bold"),
    panel.grid.minor = element_blank(),
    panel.grid.major.x = element_blank(),
    panel.grid.major.y = element_line(linetype = 3, color = "gray50"),
    axis.line.x = element_line(),
    strip.background = element_rect(fill = "gray40"),
    strip.text = element_text(color = "white"),
    legend.position = "none"
  )
```

![](team-hw2-team-hw2-9_files/figure-markdown_github/unnamed-chunk-4-1.png)

``` r
#FILTER FOR JUST STRUK BY PROSECUTION
#creating data subset
race_and_gender_prosec<- trial_juror_data %>%
  select(-c(cause_number, batson_claim_by_defense, strike_eligibility))%>%
  mutate(race = fct_collapse(race,
                             black = "Black",
                             white = "White",
                             other = c("Asian",
                                       "Latino",
                                       "Unknown"))) %>%
  filter(struck_by != "Unknown", race != "other") %>%
  filter(struck_by == "Struck by the state" | struck_by == "Juror chosen to serve on jury") %>%
  mutate(struck_by = fct_recode(struck_by,
                                "Juror Served"="Juror chosen to serve on jury"))%>%
  group_by(race) %>%
  mutate(n = n()) %>%
  group_by(race, gender) %>%
  mutate(gender_comp = n()) %>%
  group_by(race, gender, struck_by) %>%
  mutate(prop_struck = n()/gender_comp) %>%
  group_by(race, gender, struck_by, prop_struck) %>%
  summarise()

#plot
race_and_gender_prosec%>% 
  ggplot(aes(fct_reorder(race, prop_struck), prop_struck, fill = race)) +
  scale_fill_manual(values=c("black", "white")) +
  facet_grid(fct_rev(struck_by)~gender) +
  geom_bar(stat = "identity", position = position_dodge(), color = "black") +
  labs(title = "Proportion of Jurors Struck and not Struck by the Prosecution \nby Their Race and Gender",
       y = "Proportion Struck",
       x = "Race",
       caption = "*Jurors of unknown race not included") +
  theme(
    plot.background = element_rect(fill = "gray93"),
    panel.background = element_rect(fill = "gray80"),
    plot.title = element_text(face = "bold"),
    panel.grid.minor = element_blank(),
    panel.grid.major.x = element_blank(),
    panel.grid.major.y = element_line(linetype = 3, color = "gray50"),
    axis.line.x = element_line(),
    strip.background = element_rect(fill = "gray40"),
    strip.text = element_text(color = "white"),
    legend.position = "none"
  )
```

![](team-hw2-team-hw2-9_files/figure-markdown_github/unnamed-chunk-4-2.png)