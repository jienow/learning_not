官方demo
```java
import picocli.CommandLine;
import picocli.CommandLine.Command;
import picocli.CommandLine.Model.CommandSpec;
import picocli.CommandLine.Parameters;
import picocli.CommandLine.ParameterException;
import picocli.CommandLine.Spec;
import java.util.Locale;

@Command(name = "ISOCodeResolver",
  subcommands = { SubcommandAsClass.class, CommandLine.HelpCommand.class }, 
  description = "Resolves ISO country codes (ISO-3166-1) or language codes (ISO 639-1/-2)")
public class ISOCodeResolver { 
    @Spec CommandSpec spec;

    @Command(name = "country", description = "Resolves ISO country codes (ISO-3166-1)") 
    void subCommandViaMethod(
            @Parameters(arity = "1..*", paramLabel = "<countryCode>",
                  description = "country code(s) to be resolved") String[] countryCodes) {

        for (String code : countryCodes) {
            System.out.printf("%s: %s",
                    code.toUpperCase(), new Locale("", code).getDisplayCountry());
        }
    }

    public static void main(String[] args) {
        int exitCode = new CommandLine(new ISOCodeResolver()).execute(args); 
        System.exit(exitCode); 
    }
}

@Command(name = "language",
  description = "Resolves one or more ISO language codes (ISO-639-1 or 639-2)")//2
class SubcommandAsClass implements Runnable { //1

    @Parameters(arity = "1..*", paramLabel = "<languageCode>", description = "language code(s)")
    private String[] languageCodes;

    @Override
    public void run() {
        for (String code : languageCodes) {
            System.out.printf("%s: %s",
                    code.toLowerCase(), new Locale(code).getDisplayLanguage());
        }
    }
}
```
代码解释精翻：
1. 当顶层的命令并没有实现`Runnable`或者`Callable`，用户必须指定一个子命令（子命令必须是强制性的）。下面是可以选择的：如果父命令在应用程序中没有子命令而自己执行，那么他必须实现`Runnable`或`Callable`。
2. 使用`@Command`注释并且给他名字。注意到在注释中我们也能指定CommandLine.HelpCommand类作为子命令，去添加内建的`help`子命令
3. 自定义子命令可以以两种方式能够被添加进顶层命令。最简单的方式就是添加`@Command`注释给命令类。对于子命令的每个option和定位parameter，添加一个方法参数，并且使用`Option`和`@Parameters`注释来注释这些方法和参数