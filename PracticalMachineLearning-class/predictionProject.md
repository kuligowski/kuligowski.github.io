---
atitle: "Prediction project"
output: html_document
---

#How did they exercise?#

6 participant, each performing barbell lifts in a different way. Just one exercise (classe) was the correct one.

Because of the activity type participant where performing (weightlifting), I decided to use only data from forearm accelerometer. In my opinion this part of the body makes the most of motion during weigthlifting.

```{r results="hide"}
setInternet2(TRUE)
trainData <- read.csv(url("https://d396qusza40orc.cloudfront.net/predmachlearn/pml-training.csv"))
testData <- read.csv(url("https://d396qusza40orc.cloudfront.net/predmachlearn/pml-training.csv"))
```

Data clean with 0.7 NAs:
```{r}
ind <- which(colSums(is.na(testData))/nrow(testData) > 0.7)
trainDataFiltered <- trainData[,c(-1,-3,-4,-ind)]
trainDataFiltered <- trainDataFiltered[complete.cases(trainDataFiltered),]
```

Here addedd a 10-fold cross validation
```{r results="hide"}
library(caret)
tc <- trainControl("cv",10)
rpart.grid <- expand.grid(.cp=0.01)
modTree <- train(classe~., method="rpart", data=trainDataFiltered, trControl=tc, tuneGrid=rpart.grid)
```

The tree model:
```{r}
modTree
```

Below prediction with about 30% error:
```{r}
predict(modTree,newdata=Test2)
```

