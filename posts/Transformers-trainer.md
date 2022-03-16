# Trainer

Transformers 的 Trainer 将大量的工程代码进行了封装，对于搭建基线来说是一个不错的工具。在使用 Trainer 进行训练之前需要配置一个 TrainingArguments，将配置好的参数传入 Trainer 即可完成模型训练。

## TrainingArguments

### output_dir

模型预测结果和模型 checkpoint 保存文件地址。

### overwrite_output_dir

如果设置为 True ，则会覆盖输出文件中的内容。这个参数决定是否从output_dir 继续训练（当output_dir 输入参数是一个checkpoint时，则会继续进行训练）。

### do_train/do_eval/do_predict

是否进行训练/验证/预测。

### evaluation_strategy

验证时采用的策略。传入参数可以为字符串或者为 IntervalStrategy 类型数据。可以许选择的文件包括三种：“no”、“steps”、“epoch”。

* no ： 训练时不进行验证
* steps ：每 eval_steps 步进行一次验证
* epoch ： 每个 epoch 进行一次验证

### prediction_loss_only

在进行模型验证和预测时只返回loss。在使用自定义loss函数时需要设置为False。

### per_device_train_batch_size/per_device_eval_batch_size

模型训练/验证时每张卡上的 batch size 大小。

### gradient_accumulation_steps

梯度累积步数。

### eval_accumulation_steps

在将结果移动到CPU之前累计输出的预测步数。如果不进行设置，则会将整个预测在GPU上累积，最后再移动至CPU。

### learning_rate

学习率。

### weight_decay

除 AdamW 优化器的 bias 和 LayerNorm 权重以外，其他层的权重衰减。

### adam_beta1/adam_beta2/adam_epsilon

AdamW 优化器参数。

### max_grad_norm

最大梯度范数，用于梯度裁剪。

### num_train_epochs

总共训练的epoch数，支持小数输入。

### max_ratio

学习率warmup的比例。

### max_steps

默认值为-1，如果数值为正，用以设置从0到 lr 的 warmup 的步数。会覆盖 warmup_ratio 。

### log_level

在主进程上使用的日志级别。可选择的输入包括：debug、info、warning、error、critical。

### log_level_replica

在副本上使用日志的级别，和log_level一致。

### logging_dir

TensorBoard 日志地址。

### logging_strategy

日志保存策略。

* no ： 训练时不输出日志
* epoch ：在每个epoch完成记录日志
* steps： 每 logging_steps 步记录日志

### logging_first_step

是否记录和验证第一个 global_step 。

### logging_steps

当 logging_strategy=“step”时，设置步数。

### logging_nan_inf_filter

是否在日志中过滤nan和inf。

### save_strategy

训练时模型保存策略。

* no ： 训练时不进行保存
* epoch： 在每个epoch进行一次保存
* steps： 在每个 save_steps 保存一次模型

### save_steps

当 save_strategy=“steps” 时，模型保存的步数。

### save_total_limit

如果设置了数值，将会限制保存的 checkpoint 点的数量。

### save_on_each_node

在使用分布式训练时，是否在每个节点保存模型和checkpoint。

### no_cuda

是否使用cuda。

### seed

随机数。

### bf16

是否使用bf16混合精度计算。

### fp16

是否使用fp16混合精度计算。

### fp16_opt_level

使用fp16训练时，Apex AMP 优化的级别：“O0”、“O1”、“O2”、“O3”。

### half_precision_backend

混合精度计算的后端：auto、amp、apex。

### bf16_full_eval/fp16_full_eval

是否使用玩这个的bf16或者fp16计算。

### tf32

是否使用tf32模式。

### local_rank

分布式训练进程的等级。

### xpu_backend

xpu分布式训练的后端。

### tpu_num_cores

TPU训练时使用的核数。

### eval_steps

当 evaluation_strategy=“steps”时，模型验证的步数。这个数值如果没有设置的话默认跟logging_steps数值相同。

### dataloader_num_workers

使用Pytorch时，数据加载的子进程数。

### past_index

### run_name

运行时的描述符。

### disable_tqdm

### remove_unused_columns

是否删除Dataset中未使用的列。

### label_names

### load_best_model_at_end

在训练的最后是否读取最好的模型。

### metric_for_best_model

结合 load_best_model_at_end 以制定用于比较两个不同模型的指标。传入的参数必须是评估返回的指标名称。当没有指定，load_best_model_at_end=True 时，默认为loss。如果设置此值，greater_is_better 将默认为 True。

### greater_is_better

与load_best_model_at_end和metric_for_best_model配合使用。

### ignore_data_skip

当恢复训练时，是否跳过epoch和batch以获得跟之前一样阶段的数据加载。

### shareded_ddp

使用FairScale的Shared DDP训练。

### deepspeed

是否使用deepseed，传入参数时deepspeed json配置文件的位置或者已经加载的json文件。

### label_smoothing_factor

标签平滑因子。

### debug

启用一项或者多项调试功能。可以传入的选项有 underflow_overflow、tpu_metrics_debug。可以传入字符串或者列表。

### optim

优化器的选择：adam_hf、adam_torch、adam_apex_fused、adafactor。

### group_by_length

是否将训练数据中长度大致相同的样本自核在一起。

### length_column_name

### report_to

### ddp_find_unused_parameters

### ddp_bucket_cap_mb

### dataloader_pin_memory

### skip_memory_metrics

### push_to_hub

### resume_from_checkpoint

### hub_model_id

### hub_strategy

### hub_token

### gradient_checkpointing

















