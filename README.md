---
TITULO: "TAREA 04 PROGRAMACIÓN"
ALUMNO: "LIMAS CERNA ANTONIO JESUS"
CODIGO: 19160180
FECHA: "04/09/2021"
---

## APLICACIÓN EN SHINY

```{shiny}
library(shiny)
library(shinydashboard)
library(sp)
library(sf)
library(leaflet)

setwd("D:/Antonio/Downloads/SHINY")
getwd()

ter <- read.csv("D:/Antonio/Downloads/SHINY/earthquakes_.csv")

ui <- dashboardPage(
    dashboardHeader(title = "TAREA 04"),
    dashboardSidebar(
        sidebarMenu(
            menuItem(text = "Mapa", tabName = "Mapa"),
            menuItem(text = "Tabla", tabName = "Tabla"),
            menuItem(text = "Diagrama", tabName = "Diagrama")
            
        )
    ),
    dashboardBody(
        tabItems(
            tabItem(
                "Mapa",
                leafletMap(outputId = "leaflet")))
        ),
        tabItem(
            "Tabla",
            fluidPage(
                dataTableOutput("tabla"))),

    tabItem(
        "Diagrama",
        box(plotOutput(outputId = "diagram")),
        box(selectInput(inputId = "variable", "Seleccionar variable:", 
                        c ("Latitude", "Longitude", "DateTime")))
    )
    )
    

server <- function(input, output){
    output$leaflet <- renderPlot({
        leaflet() %>% addTiles() %>%
            addMarkers(data=ter, lat = ~Latitude, lng = ~Longitude,
                       popup = paste0("Magnitud:", 
                                      as.character(ter$Magnitude)))   
    })
    output$diagram <- renderPlot({
        plot(ter$Magnitude, ter[,input$variable])
    })
    output$tabla <- renderDataTable(ter)
    
}


shinyApp(ui,server)
```
