这个是file flush的意思

就是在我们输入的时候,如果接下来的输入,我们想要处理掉的话,就要用flush来处理,经常用到的场合就是一个空格键或者是换行符吗没有被读掉,这时就要用 fflush来处理掉换行符

```c
#include <stdio.h>
#include <conio.h>
 
void main( void )
{
   int integer;
   char string[81];
 
   /* Read each word as a string. */
   printf( "Enter a sentence of four words with scanf: " );
   for( integer = 0; integer < 4; integer++ )
   {
      scanf( "%s", string );
      printf( "%s\n", string );
   }
 
   /* You must flush the input buffer before using gets. */
   fflush( stdin );
   printf( "Enter the same sentence with gets: " );
   gets( string );
   printf( "%s\n", string );
}

```

如果不用fflush的话,那么接下来的gets取到的是一个换行符,



如果对象是以写入方式打开的话,那么就会强制将信息输出到文件中去,一般有缓冲的文件写入的话,一般要遇到换行符或者是缓冲区满了,才能将将缓存输出到文件的.