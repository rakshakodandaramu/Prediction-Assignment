### Loading Data

we have deleted thoe coloumns with NA (nothing) and few more coloumns which not related to predict split data to training and testing

```
rm(list = ls(all = TRUE))

library(caret)
set.seed(123)

training <- read.csv("C:/Course Work/Machine learning/Courseera/R/machine learnin course/pml-training.csv")

testing<-read.csv("C:/Course Work/Machine learning/Courseera/R/machine learnin course/pml-testing.csv")

inTrain <- createDataPartition(y=training$classe, p=0.7, list=F)
training1 <- training[inTrain, ]
training2 <- training[-inTrain, ]
```

## preprocessing data
```
near-zero variance predictors
neartoz <- nearZeroVar(training)
training1 <- training1[, -neartoz]
training2 <- training2[, -neartoz]



removing with NA values
training1 <- training1[, colSums(is.na(training1)) == 0]
training2 <- training2[, colSums(is.na(training2)) == 0]


removing columns not related
training1 <- training1[, -(1:5)]
training2 <- training2[, -(1:5)]

```

## Model fit
```
mod <- train(classe ~., method = "rf", data = training1, verbose = TRUE, trControl = trainControl(method="cv"), number = 3)
pred1 <- predict(mod, training1)
confusionMatrix(pred1, training1$classe)

pred2 <- predict(mod, training2)
confusionMatrix(pred2, training2$classe)
```

## Testing Fit or cross Validation
```
# Apply same as on training                
testing <- testing[, colSums(is.na(testing)) == 0]
testing <- testing[, -(1:5)]
nzvt <- nearZeroVar(testing)
testing <- testing[, -nzvt]
pred3 <- predict(mod, testing)
pred3
```