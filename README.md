### xgettext 
author: LiXizhi
date:2014.11.23

### Source Code
xgettext is a cool tool for automatically extracting text from source code. 
I modified the source code of gettext to make it support embedded code in MCML .

see x-lua.c
```c
		case '%':
		{
			{
				// modified by LiXizhi 2014.11.23, skipping mcml style string %>'"
				int ahead_c = phase1_getc();
				if (ahead_c == '>')
				{
					ahead_c = phase1_getc();
					if (ahead_c == '\'' || ahead_c == '"')
					{
						// eat all mcml %>' or  %>"
						continue;
					}
					else
						phase1_ungetc(ahead_c);
				}
				else
					phase1_ungetc(ahead_c);
			}
			tp->type = token_type_operator1;
			return;
		}
        case '"':
		case '\'':
		{
			// modified by LiXizhi 2014.11.23, skipping mcml style string "<%
			int ahead_c = phase1_getc();
			if (ahead_c == '<')
			{
				ahead_c = phase1_getc();
				if (ahead_c == '%')
				{
					// eat all mcml "<% or '<%
					continue;
				}
				else
					phase1_ungetc(ahead_c);
			}
			else
				phase1_ungetc(ahead_c);
		}
```

### Install 
1. install CYGWin or copy its dll. 
2. copy all xgettext.exe files to Poedit/GettextTools/bin/ folder (replace old file)
3. run Poedit and add parser rules with *.lua;*.html;*.xml;

### Build
1. install cygwin and packages: `gcc`, make, autoconf, `libiconv`
2. replace x-lua.c with the new one. 
3. build gettext with cygwin.
4. deploy xgettext.exe to poedit/GettextTools/bin/ folder. 


