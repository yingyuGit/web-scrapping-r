# web-scrapping-r
R web scrapping: scrape recipes

```
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
