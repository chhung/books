# Log4j2

## Maven

{% code-tabs %}
{% code-tabs-item title="pom.xml" %}
```markup
<!-- https://mvnrepository.com/artifact/org.apache.logging.log4j/log4j-core -->
<dependency>
	<groupId>org.apache.logging.log4j</groupId>
	<artifactId>log4j-core</artifactId>
	<version>2.12.1</version>
</dependency>

<!-- https://mvnrepository.com/artifact/org.apache.logging.log4j/log4j-api -->
<dependency>
	<groupId>org.apache.logging.log4j</groupId>
	<artifactId>log4j-api</artifactId>
	<version>2.12.1</version>
</dependency>
```
{% endcode-tabs-item %}
{% endcode-tabs %}

## 配置檔設定，放在src/main/resources目錄下，log4j自動會去找

{% code-tabs %}
{% code-tabs-item title="log4j2.xml" %}
```markup
<?xml version="1.0" encoding="UTF-8"?>

<!-- 優先等級: OFF > FATAL > ERROR > WARN > INFO > DEBUG > TRACE > ALL -->
<!--Configuration後面的status(status="Debug"),這個用於設置log4j2自身內部的信息輸出,可以不設置,當設置成trace時,你會看到log4j2內部各種詳細輸出 -->
<!--monitorInterval：Log4j能夠自動檢測修改配置 文件和重新配置本身,設置間隔秒數 -->
<Configuration status="OFF" monitorInterval="30">
	<!-- 變數定義 -->
	<properties>
		<property name="LOG_HOME">/log/mis</property>
		<property name="FILE_NAME">mis</property>
	</properties>

	<!--定義appender -->
	<Appenders>
		<!-- 用來定義輸出到控制檯的配置 -->
		<Console name="STDOUT" target="SYSTEM_OUT">
			<PatternLayout
				pattern="%d %-5p [%t] %C{2} (%F:%L) - %m%n" />
		</Console>
		<File name="FILE" fileName="${LOG_HOME}/logfile_fileMode.log"
			append="true">
			<PatternLayout
				pattern="%d %-5p [%t] %C{2} (%F:%L) - %m%n" />
		</File>

		<!-- 打印root中指定的level級別以上的日誌到文件 -->
		<RollingRandomAccessFile name="MIS_PROJECT"
			fileName="${LOG_HOME}/${FILE_NAME}.log"
			filePattern="${LOG_HOME}/$${date:yyyy-MM}/${FILE_NAME}-%d{yyyy-MM-dd}-%i.log.gz">

			<PatternLayout
				pattern="%d %-5p [%t] %C{2} (%F:%L) - %m%n" />
			<Policies>
				<TimeBasedTriggeringPolicy />
				<!-- 單一檔最大為10MB -->
				<SizeBasedTriggeringPolicy size="10 MB" />
			</Policies>
			<!-- 壓縮包最多保留20個 -->
			<DefaultRolloverStrategy max="20" />
		</RollingRandomAccessFile>

	</Appenders>

	<Loggers>
		<!--过滤掉spring和mybatis的一些无用的DEBUG信息 -->
		<logger name="org.springframework" level="INFO"></logger>
		<logger name="org.mybatis" level="INFO"></logger>

		<Root level="debug">
			<AppenderRef ref="STDOUT" />
		</Root>
		<!-- <Root level="WARN"> <AppenderRef ref="MIS_PROJECT" /> </Root> -->


	</Loggers>
</Configuration>

```
{% endcode-tabs-item %}
{% endcode-tabs %}

## Use it

```java
import org.apache.logging.log4j.LogManager;
import org.apache.logging.log4j.Logger;

public class App {
	private static final Logger log = LogManager.getLogger(App.class);
	
	public static void main(String[] args) {
		log.info("Hello Log");
	}
}
```

