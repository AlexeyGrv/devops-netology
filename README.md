
Игнорируются любые локальные каталоги .terraform
**/.terraform/*

Игнорируются все файлы с расширением, в том числе без имени .tfstate и любыми символами после расширения tfstate. 
*.tfstate
*.tfstate.*

Игнорируются все файлы, в любом каталоге
crash.log

Игнорируются все файлы c расширением .tfvars , в любом каталоге 
*.tfvars

Игнорируются все файлы, в любом каталоге  override.tf, override.tf.json,
а так же файлы с любым количеством символов перед _override.tf и _override.tf.json

Игнорируются все файлы
.terraformrc
terraform.rc для Win
New line
