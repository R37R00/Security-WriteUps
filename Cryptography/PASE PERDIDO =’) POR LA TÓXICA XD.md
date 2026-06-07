# CIDSI presents: PASE PERDIDO =’) POR LA TÓXICA XD(80pts.)
## Description
Pablito faltó a clases y quiere participar en la 4 Competencia de Seguridad Informática CIDSI, uno de sus docentes le dejo una pista, será que Pablito participa de la Competencia??? 494a55574b3354574d565847535a44504c354255535243544a465054454d425347493d3d3d3d3d3d

### Steps to solve the challenge:
1. At first look at the weird string **494a55574b3354574d565847535a44504c354255535243544a465054454d425347493d3d3d3d3d3d** this is hexadecimal encoding or often called hex.
#### What is hexadecimal encoding?
Is a way to represent data using the base-16 number system instead of normal decimal(base-10). It uses 16 symbols:
**0 1 2 3 4 5 6 7 8 9 A B C D E F**
2. The weird string only contains numbers 0-9 and letters A-F, so thats why we infer is hex coding.
3. So now use any hex decoder of your preference.
**Note:** In order to learn linux commands I'm using the next:
4. Use this command to decode hex:
```
echo "494a55574b3354574d565847535a44504c354255535243544a465054454d425347493d3d3d3d3d3d" | xxd -r -p
```
and we'll get:
IJUWK3TWMVXGSZDPL5BUSRCTJFPTEMBSGI======
This is base32 encoding.
### What is base32 encoding?
Is a way to encode binary data intro text using 32 characters:
A-Z and 2-7
5. Decode it using the next command:
```bash
echo -n "IJUWK3TWMVXGSZDPL5BUSRCTJFPTEMBSGI======" | base32 -d

```
6. There we'll have a decoded text:

"Bienvenido_CIDSI_2022"
7. Hash it to MD5 and there you have the answer!!!
