# CIDSI presents: Movimientos(150pts.)
## Description:
Desempolvando los pasos desde los 90'. formato de la flag cidsi{flag_en_md5}
There is an image attached to the challenge.

### Steps to solve:
1. The image is a sequence of 10 stickmen on different posses.
2. This is the dancing man cipher which is a kind of Sherlock Holmes' reference.
3. Use this site to decrypt it [site](https://www.dcode.fr/dancing-men-cipher#q1)
4. The string you get is disordered:

**"REKLAWNOOM"**
5. The hint says "Pasos de los 90s", so if we order the string it says:

**"MOONWALKER"**
6. But thats not the answer, use lowercase letters:
```
echo -n "moonwalker" | md5sum
```
And there you have the flag!!!
