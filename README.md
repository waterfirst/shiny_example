# shiny_example

library(shiny)

library(tidyverse)

ui <- fluidPage(
  selectInput("dataset", label = "Dataset", choices = ls("package:datasets")), #데이터 선택 박스

  verbatimTextOutput("summary"), #서머리 결과 텍스트 박스
  tableOutput("table"),#데이터 테이블
  plotOutput("histogram"),#plot output
  
  
  numericInput("age", "How old are you?", value = NA),#Input 숫자
  textInput("name", "What's your name?"), #Input 문자
  
  textOutput("greeting") #텍스트 output
  
)

server <- function(input, output, session) {

   dataset <- reactive({
    get(input$dataset, "package:datasets")
  })
  
    output$summary <- renderPrint({              #서머리 output
    summary(dataset()) #서머리를 보여줘라
  })
  
  output$table <- renderTable({ #데이터 테이블
    dataset <- get(input$dataset, "package:datasets") #데이터는 Input 데이터 선택에서 받아와라
    dataset#데이터
  })
  
  output$histogram <- renderPlot({ #Plot output
    hist(rnorm(1000)) #histogram
  }, res = 96)
  
  output$greeting <- renderText({ #텍스트 output + Input Name
    paste0("Hello ", input$name, ".\n  You are ", input$age, "years old.")
  })
  

  
}


shinyApp(ui, server)

rsconnect::setAccountInfo(name='waterfirst76',
                          token='B3A23E23B5457D6F5BBF8DEFBC8450A7',
                          secret='<SECRET>')
