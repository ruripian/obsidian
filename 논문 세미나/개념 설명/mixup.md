
### Regularization 기법 중 하나 

모델을 학습할 때 Overfitting을 방지하기 위해 다양한 규제(Regularization) 기법이 존재합니다.

Mixup은 그 중 데이터 증강(Data Augmentation)과 관련된 기술 중 하나로, 학습 데이터에서 두 개의 샘플 데이터를 혼합(Mix)하여 새로운 학습 데이터를 만드는 기술입니다.

![[Pasted image 20231010182235.png]]

![[Pasted image 20231010182251.png]]

### Mixup Input & Label Data

```python
def MixUp(input, target, alpha=1.0):
	if CFG['ALPHA'] > 0:
		lambda_ = np.random.beta(alpha, alpha)
	else:
		lambda_ = 1
	
	batch_size = input.size(0)
	index = torch.randperm(batch_size)
	
	mixed_input = lambda_ * input + (1 - lambda_) * input[index, :]
	labels_a, labels_b = target, target[index]
	
	return mixed_input, labels_a, labels_b, lambda_
	```

### Mixup Loss

```python
def MixUpLoss(criterion, pred, labels_a, labels_b, lambda_):
	return lambda_ * criterion(pred, labels_a) + (1 - lambda_) * criterion(pred, labels_b)
```

### Train 예시

Mixup을 사용한다면, Batch 3번째 마다 Mixup을 적용하여 preds 값을 예측 후 Mixup Loss를 구합니다.

```python
...
criterion = nn.CrossEntropyLoss().to(device)
...

	for epoch in range(1, epochs+1):    
		self.model.train()
		
		train_loss = []
		for idx, (imgs, labels) in enumerate(tqdm(iter(train_dataloader))):
			imgs = imgs.float().to(device)
			labels = labels.to(device)
			
			self.optimizer.zero_grad()
			
			if CFG['MIXUP'] and (idx + 1) % 3 == 0:
				imgs, labels_a, labels_b, lambda_ = MixUp(imgs, labels)
				output = model(imgs)
				loss = MixUpLoss(criterion, pred=output, labels_a_a=labels_a, labels_b=labels_b, lambda_=lambda_)
			else:
				output = model(imgs)
				loss = criterion(output, labels)
				
			loss.backward()
			optimizer.step()
			
			train_loss.append(loss.item())
			
	_val_loss, _val_acc = self.validation()
	_train_loss = np.mean(train_loss)
```