# bsdiff-4.3

### Download from `http://www.daemonology.net/bsdiff/`

#### Decompression
1,decompression bsdiff-4.3.

2,`cd bsdiff-4.3` and make.show first error.

```
$ make
Makefile:13: *** missing separator.  Stop.
```

3,modify `Makefile` file format.

```c
CFLAGS		+=	-O3 -lbz2

PREFIX		?=	/usr/local
INSTALL_PROGRAM	?=	${INSTALL} -c -s -m 555
INSTALL_MAN	?=	${INSTALL} -c -m 444

all:		bsdiff bspatch
bsdiff:		bsdiff.c
bspatch:	bspatch.c

install:
	${INSTALL_PROGRAM} bsdiff bspatch ${PREFIX}/bin
	.ifndef WITHOUT_MAN
	${INSTALL_MAN} bsdiff.1 bspatch.1 ${PREFIX}/man/man1
	.endif
```

4,continual `make`,show fuck error again.

```c
bspatch.c:39:21: error: unknown type name 'u_char'; did you mean 'char'?
static off_t offtin(u_char *buf)
                    ^~~~~~
                    char
bspatch.c:65:8: error: expected ';' after expression
        u_char header[32],buf[8];
              ^
              ;
bspatch.c:65:2: error: use of undeclared identifier 'u_char'; did you mean
      'putchar'?
        u_char header[32],buf[8];
        ^~~~~~
        putchar
/usr/include/stdio.h:261:6: note: 'putchar' declared here
int      putchar(int);
         ^
bspatch.c:65:9: error: use of undeclared identifier 'header'
        u_char header[32],buf[8];
               ^
bspatch.c:65:20: error: use of undeclared identifier 'buf'
        u_char header[32],buf[8];
                          ^
bspatch.c:66:2: error: use of undeclared identifier 'u_char'; did you mean
      'putchar'?
        u_char *old, *new;
        ^~~~~~
        putchar
/usr/include/stdio.h:261:6: note: 'putchar' declared here
int      putchar(int);
         ^
bspatch.c:66:10: error: use of undeclared identifier 'old'
        u_char *old, *new;
                ^
bspatch.c:66:16: error: use of undeclared identifier 'new'
        u_char *old, *new;
                      ^
bspatch.c:93:12: error: use of undeclared identifier 'header'
        if (fread(header, 1, 32, f) < 32) {
                  ^
bspatch.c:100:13: error: use of undeclared identifier 'header'
        if (memcmp(header, "BSDIFF40", 8) != 0)
                   ^
bspatch.c:104:19: error: use of undeclared identifier 'header'
        bzctrllen=offtin(header+8);
                         ^
bspatch.c:105:19: error: use of undeclared identifier 'header'
        bzdatalen=offtin(header+16);
                         ^
bspatch.c:106:17: error: use of undeclared identifier 'header'
        newsize=offtin(header+24);
                       ^
bspatch.c:137:5: error: use of undeclared identifier 'old'
                ((old=malloc(oldsize+1))==NULL) ||
                  ^
bspatch.c:139:12: error: use of undeclared identifier 'old'
                (read(fd,old,oldsize)!=oldsize) ||
                         ^
bspatch.c:141:6: error: use of undeclared identifier 'new'
        if((new=malloc(newsize+1))==NULL) err(1,NULL);
            ^
bspatch.c:147:43: error: use of undeclared identifier 'buf'
                        lenread = BZ2_bzRead(&cbz2err, cpfbz2, buf, 8);
                                                               ^
bspatch.c:151:19: error: use of undeclared identifier 'buf'
                        ctrl[i]=offtin(buf);
                                       ^
bspatch.c:159:42: error: use of undeclared identifier 'new'
                lenread = BZ2_bzRead(&dbz2err, dpfbz2, new + newpos, ctrl[0]);
                                                       ^
fatal error: too many errors emitted, stopping now [-ferror-limit=]
20 errors generated.
make: *** [bspatch] Error 1

```

5,fix error, inclued code be `bspatch.c`.

```
#include <sys/types.h>
```

6,OK , `make` .auto create file `bsdiff` and `bspatch`.

7, build `patchfile`.

```
./bsdiff old.apk new.apk old-to-new.patch
```

8, build `newFile`

```
./bsdiff old.apk new1.apk old-to-new.patch
```

9, check `new1.apk`

```
md5 new.apk
md5 new1.apk
```

10,done.