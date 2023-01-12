参考 [[[HFCTF2020]EasyLogin]]

`node.js`的`jwt.verify`在`secret`为空时，无视第三个参数，转而采用`none`作为签名算法，因而给`jwt`的伪造创造了机会

#Web #nodejs #jwt 