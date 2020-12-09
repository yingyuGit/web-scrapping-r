# web-scrapping-r
R web scrapping: scrape recipes

### Scrap recipes from Foodnetwork website
```
library(rvest)

# get recipe page links from a main list page
home <- read_html("http://www.foodnetwork.com/recipes/food-network-kitchen/pancakes-recipe-1913844")
recipe_links <- home %>% html_nodes(".recipepage .card-link") %>% html_attr("href")
recipe_links <- paste("https://foodnetwork.co.uk", recipe_links)
recipe_links = gsub(" ", "", recipe_links)

# create an empty data frame to store the data later
recipes = data.frame()

# get recipe info from each recipe page and store it to the recipes data frame
for (link in recipe_links[1:3]) {
    page = read_html(link)
    recipe <- page %>% html_node(".recipe-text") %>% html_text()
    prep_time <- page %>% html_node("ul.recipe-head > li:nth-child(1) > strong") %>% html_text()
    ingredients <- page %>% html_nodes(".recipe-tab-container > .ingredient") %>% html_text() %>% paste(collapse = ",")
    recipes = rbind(recipes, data.frame(recipe, prep_time, ingredients, stringsAsFactors = FALSE)) 
    print(paste("Pages", link))
}

# export data to csv format
write.csv(recipes, "yourpath/recipes.csv")

```

### Other useful notes: Scrap tables 
* it is not related to the recipe scrapping that has been mentioned above.
```
# import a webpage into R
library(rvest)
url <- "insert-your-url"
page <- read_html(url)

# get all the table elements and then pick second table(xml_node)
tab <- page %>% html_nodes("table")
tab <- tab[[2]]

# change a xml_node to a data frame
tab <- tab %>% html_table

# set columns names
tab <- tab %>% setNames(c("A", "B", "C"))
```
