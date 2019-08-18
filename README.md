# HotdogOrNot
HotdogOrNot from https://www.kaggle.com/c/hotdogornot

| Model Number  | Local CV F1 Validation| Private LB F1       | Private LB F1 with TTA  | Public LB F1        | Public LB F1 with TTA  |
| ------------- | ----------------------|---------------------|-------------------------|---------------------|------------------------|
|   7           | 0.987538              | **0.97435**         | 0.96268                 | 0.96190             | 0.97560                |
|   6           | 0.971428              | 0.96461             | 0.95238                 | 0.97607             | 0.94835                |
|   5           | 0.968698              | 0.96216             | 0.96202                 | 0.96682             | 0.97169                |
|   8           | 0.992805              | 0.96160             | 0.94074                 | 0.98076             | 0.96226                |
|   2           |                       | 0.95970             | 0.95970                 | **0.98564**         | 0.98076                |
|   3           |                       | 0.95825             | 0.96118                 | 0.95693             | 0.96618                |
|   1           |                       | 0.95429             | 0.94871                 | 0.94685             | 0.95693                |
|   4           |                       | 0.95202             | 0.94559                 | 0.95145             | 0.95609                |
|   0           |                       | 0.90400             |                         | 0.91752             |                        |

## Models
0. Benchmark: Untrained model resnet101
1. Base model

		○ Resnet101, trainable fc-layer: model.fc = nn.Linear(model.fc.in_features, 2)
		○ Image size: 224x224
		○ Optimizer = optim.SGD
		○ LR: fc=8e-3 layer4=8e-4 layer3=8e-5 
		○ weight_decay=0.05
		○ momentum=0.9 
		○ loss = nn.CrossEntropyLoss() 
		○ Augmentations: RandomHorizontalFlip
2. Base model with a second trainable FCL + Dropout

		○ Model1
		○ nn.Linear(model.fc.in_features, 128)->ReLU(inplace=True)->Dropout(p=0.1)->Linear(128, 2)
3. Base model with cosine LR scheduler

		○ Model1
		○ CosineAnnealingLR(optimizer, eta_min=0.0001, T_max=5, last_epoch=-1)
4. ResNeXt-50

		○ Base model with ResNeXt-50
5. Base model with Image size 512x512

		○ Model1
		○ Batch size=8
		○ Image size: 512x512
		○ LR: fc=8e-3 layer4=8e-4 layer3=8e-5. After 10 epoch lr decreases by 10 times
		○ Augmentations: RandomHorizontalFlip
6. Model5 with RandomGrayscale

		○ Model5
		○ Augmentations: RandomHorizontalFlip, RandomGrayscale
7. Model5 with cosine annealing

		○ Model5
		○ 50 epoch, initial lr=1e-2, eta_min=1e-6, T_max=10. Then pick the best model and another cosine annealing and initial lr= 8e-4, eta_min=8e-6
8. Model5 with cosine annealing and more Augmentations

		○ Model5
		○ 50 epoch, initial lr=0.01, eta_min=0.000001, T_max=10
		○ Augmentations: RandomHorizontalFlip, RandomGrayscale, RandomRotation(-10,10), ColorJitter(brightness, contrast, hue, saturation=0.2)

## Learning rate range test 
### Train&Val history according to lr

![](https://raw.githubusercontent.com/basic39/HotdogOrNot/master/images/TrainVal_lr.png)

### Loss history according to lr

![](https://raw.githubusercontent.com/basic39/HotdogOrNot/master/images/Loss_value_lr.png)
