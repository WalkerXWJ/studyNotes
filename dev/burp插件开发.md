#burp-Extender-APIs 
# API
v 2025.6.5
### BurpExtension
```java
/*
 * 版权所有 (c) 2022-2023。PortSwigger Ltd. 保留所有权利。
 *
 * 此代码可用于扩展Burp Suite社区版和Burp Suite专业版的功能，
 * 前提是该使用不违反这些产品的许可条款。
 */

package burp.api.montoya;

import java.util.Set;

import static java.util.Collections.emptySet;

/**
 * 所有Burp扩展必须实现的接口。
 * <p>
 * 实现类必须声明为public，并且必须提供默认的public无参构造函数。
 */
public interface BurpExtension
{
    /**
     * 当扩展被加载时调用。所有注册的处理器将在此方法完成后启用。
     *
     * @param api 用于访问Burp Suite功能的API实现
     */
    void initialize(MontoyaApi api);

    /**
     * 当扩展被加载时调用，用于确定是否需要任何增强功能。
     *
     * @see EnhancedCapability
     * @return 所需增强功能集合
     */
    default Set<EnhancedCapability> enhancedCapabilities()
    {
        return emptySet();
    }
}
```
### EnhancedCapability
```java
/*
 * 版权所有 (c) 2022-2024。PortSwigger Ltd. 保留所有权利。
 *
 * 此代码可用于扩展Burp Suite社区版和Burp Suite专业版的功能，
 * 前提是该使用不违反这些产品的许可条款。
 */

package burp.api.montoya;

/**
 * 需要显式请求的增强功能枚举。
 */
public enum EnhancedCapability
{
    /**
     * AI相关功能增强
     */
    AI_FEATURES
}

```
### MontoyaApi
```java
/*
 * 版权所有 (c) 2022-2023。PortSwigger Ltd. 保留所有权利。
 *
 * 此代码可用于扩展Burp Suite社区版和Burp Suite专业版的功能，
 * 前提是该使用不违反这些产品的许可条款。
 */

package burp.api.montoya;

import burp.api.montoya.ai.Ai;
import burp.api.montoya.bambda.Bambda;
import burp.api.montoya.burpsuite.BurpSuite;
import burp.api.montoya.collaborator.Collaborator;
import burp.api.montoya.comparer.Comparer;
import burp.api.montoya.decoder.Decoder;
import burp.api.montoya.extension.Extension;
import burp.api.montoya.http.Http;
import burp.api.montoya.intruder.Intruder;
import burp.api.montoya.logging.Logging;
import burp.api.montoya.organizer.Organizer;
import burp.api.montoya.persistence.Persistence;
import burp.api.montoya.project.Project;
import burp.api.montoya.proxy.Proxy;
import burp.api.montoya.repeater.Repeater;
import burp.api.montoya.scanner.Scanner;
import burp.api.montoya.scope.Scope;
import burp.api.montoya.sitemap.SiteMap;
import burp.api.montoya.ui.UserInterface;
import burp.api.montoya.utilities.Utilities;
import burp.api.montoya.websocket.WebSockets;

/**
 * Burp Suite扩展API主接口，提供访问所有Burp功能的入口点。
 * <p>
 * 当扩展被加载时，Burp会调用其{@link BurpExtension#initialize(MontoyaApi)}方法，
 * 并传入{@link MontoyaApi}接口实例。扩展随后可以根据需要调用此接口的方法来扩展Burp的功能。
 */
public interface MontoyaApi
{
    /**
     * [仅限专业版] 访问AI相关功能。
     * <p>注意：扩展必须通过{@link BurpExtension#enhancedCapabilities()}声明需要AI功能。</p>
     *
     * @return 提供AI功能的接口实现
     */
    Ai ai();

    /**
     * 访问Bambda相关功能。
     *
     * @return 提供Bambda功能的接口实现
     */
    Bambda bambda();

    /**
     * 访问Burp Suite应用程序级别功能。
     *
     * @return 提供应用级功能的接口实现
     */
    BurpSuite burpSuite();

    /**
     * [仅限专业版] 访问Collaborator功能。
     *
     * @return 提供Collaborator功能的接口实现
     */
    Collaborator collaborator();

    /**
     * 访问Comparer功能。
     *
     * @return 提供Comparer功能的接口实现
     */
    Comparer comparer();

    /**
     * 访问Decoder功能。
     *
     * @return 提供Decoder功能的接口实现
     */
    Decoder decoder();

    /**
     * 访问扩展相关功能。
     *
     * @return 提供扩展功能的接口实现
     */
    Extension extension();

    /**
     * 访问HTTP请求和响应相关功能。
     *
     * @return 提供HTTP功能的接口实现
     */
    Http http();

    /**
     * 访问Intruder功能。
     *
     * @return 提供Intruder功能的接口实现
     */
    Intruder intruder();

    /**
     * 访问日志和事件相关功能。
     *
     * @return 提供日志功能的接口实现
     */
    Logging logging();

    /**
     * 访问Organizer功能。
     *
     * @return 提供Organizer功能的接口实现
     */
    Organizer organizer();

    /**
     * 访问持久化相关功能。
     *
     * @return 提供持久化功能的接口实现
     */
    Persistence persistence();

    /**
     * 访问项目相关功能。
     *
     * @return 提供项目功能的接口实现
     */
    Project project();

    /**
     * 访问Proxy功能。
     *
     * @return 提供Proxy功能的接口实现
     */
    Proxy proxy();

    /**
     * 访问Repeater功能。
     *
     * @return 提供Repeater功能的接口实现
     */
    Repeater repeater();

    /**
     * [仅限专业版] 访问Scanner功能。
     *
     * @return 提供Scanner功能的接口实现
     */
    Scanner scanner();

    /**
     * 访问Burp套件范围的目标scope功能。
     *
     * @return 提供scope功能的接口实现
     */
    Scope scope();

    /**
     * 访问Site Map功能。
     *
     * @return 提供站点地图功能的接口实现
     */
    SiteMap siteMap();

    /**
     * 访问用户界面相关功能。
     *
     * @return 提供用户界面功能的接口实现
     */
    UserInterface userInterface();

    /**
     * 访问其他实用工具功能。
     *
     * @return 提供实用工具的接口实现
     */
    Utilities utilities();

    /**
     * 访问WebSocket及相关消息功能。
     *
     * @return 提供WebSocket功能的接口实现
     */
    WebSockets websockets();
}
```
## ai
### Ai
```java

/*
 * 版权所有 (c) 2022-2024。PortSwigger Ltd. 保留所有权利。
 *
 * 此代码可用于扩展Burp Suite社区版和Burp Suite专业版的功能，
 * 前提是该使用不违反这些产品的许可条款。
 */

package burp.api.montoya.ai;

import burp.api.montoya.ai.chat.Prompt;

/**
 * 提供访问AI相关功能的接口。
 */
public interface Ai
{
    /**
     * 检查扩展是否有权限使用AI功能。
     *
     * @return 如果扩展可以使用AI功能返回true，否则返回false
     */
    boolean isEnabled();

    /**
     * 访问AI聊天提示相关功能。
     *
     * @return 提供聊天提示功能的Prompt接口实现
     */
    Prompt prompt();
}
```
### chat
#### Message
```java
/*
 * 版权所有 (c) 2022-2024。PortSwigger Ltd. 保留所有权利。
 *
 * 此代码可用于扩展Burp Suite社区版和Burp Suite专业版的功能，
 * 前提是该使用不违反这些产品的许可条款。
 */

package burp.api.montoya.ai.chat;

import static burp.api.montoya.internal.ObjectFactoryLocator.FACTORY;

/**
 * 表示AI聊天提示中使用的不同类型消息的接口。
 */
public interface Message
{
    /**
     * 创建系统消息。
     *
     * @param content 消息内容
     * @return 消息对象
     */
    static Message systemMessage(String content)
    {
        return FACTORY.systemMessage(content);
    }

    /**
     * 创建用户消息。
     *
     * @param content 消息内容
     * @return 消息对象
     */
    static Message userMessage(String content)
    {
        return FACTORY.userMessage(content);
    }

    /**
     * 创建助手消息。
     *
     * @param content 消息内容
     * @return 消息对象
     */
    static Message assistantMessage(String content)
    {
        return FACTORY.assistantMessage(content);
    }
}
```
#### Prompt
```java
/*
 * 版权所有 (c) 2022-2024。PortSwigger Ltd. 保留所有权利。
 *
 * 此代码可用于扩展Burp Suite社区版和Burp Suite专业版的功能，
 * 前提是该使用不违反这些产品的许可条款。
 */

package burp.api.montoya.ai.chat;

/**
 * 提供AI聊天提示功能的接口。
 */
public interface Prompt
{
    /**
     * 使用AI聊天提示评估一系列消息。
     *
     * @param messages 要评估的消息数组
     * @return 包含聊天提示响应的{@link PromptResponse}对象
     * @throws PromptException 如果执行提示时出现问题
     */
    PromptResponse execute(String... messages) throws PromptException;

    /**
     * 使用指定的提示选项评估一系列消息。
     *
     * @param options 提示选项
     * @param messages 要评估的消息数组
     * @return 包含聊天提示响应的{@link PromptResponse}对象
     * @throws PromptException 如果执行提示时出现问题
     */
    PromptResponse execute(PromptOptions options, String... messages) throws PromptException;

    /**
     * 使用AI聊天提示评估一系列消息对象。
     *
     * @param messages 要评估的{@link Message}对象数组
     * @return 包含聊天提示响应的{@link PromptResponse}对象
     * @throws PromptException 如果执行提示时出现问题
     */
    PromptResponse execute(Message... messages) throws PromptException;

    /**
     * 使用指定的提示选项评估一系列消息对象。
     *
     * @param options 提示选项
     * @param messages 要评估的{@link Message}对象数组
     * @return 包含聊天提示响应的{@link PromptResponse}对象
     * @throws PromptException 如果执行提示时出现问题
     */
    PromptResponse execute(PromptOptions options, Message... messages) throws PromptException;
}
```
#### PromptException
```java
/*
 * 版权所有 (c) 2022-2024。PortSwigger Ltd. 保留所有权利。
 *
 * 此代码可用于扩展Burp Suite社区版和Burp Suite专业版的功能，
 * 前提是该使用不违反这些产品的许可条款。
 */

package burp.api.montoya.ai.chat;

/**
 * 表示在使用AI聊天提示功能时可能抛出的异常。
 */
public class PromptException extends RuntimeException
{
    /**
     * 使用指定的错误消息构造PromptException。
     *
     * @param message 错误描述信息
     */
    public PromptException(String message)
    {
        super(message);
    }
}
```
#### PromptOptions
```java
/*
 * 版权所有 (c) 2022-2024。PortSwigger Ltd. 保留所有权利。
 *
 * 此代码可用于扩展Burp Suite社区版和Burp Suite专业版的功能，
 * 前提是该使用不违反这些产品的许可条款。
 */

package burp.api.montoya.ai.chat;

import static burp.api.montoya.internal.ObjectFactoryLocator.FACTORY;

/**
 * 用于配置AI聊天提示选项的接口。
 */
public interface PromptOptions
{
    /**
     * 设置提示温度参数。
     *
     * @param temperature 温度值(通常0.0-2.0)
     * @return 配置后的PromptOptions实例
     */
    PromptOptions withTemperature(double temperature);

    /**
     * 创建默认PromptOptions实例。
     *
     * @return 新的PromptOptions实例
     */
    static PromptOptions promptOptions()
    {
        return FACTORY.promptOptions();
    }
}
```
#### PromptResponse
```java
/*
 * 版权所有 (c) 2022-2024。PortSwigger Ltd. 保留所有权利。
 *
 * 此代码可用于扩展Burp Suite社区版和Burp Suite专业版的功能，
 * 前提是该使用不违反这些产品的许可条款。
 */

package burp.api.montoya.ai.chat;

/**
 * 表示AI聊天提示的响应结果。
 */
public interface PromptResponse
{
    /**
     * 获取AI生成的响应内容。
     *
     * @return 响应内容字符串
     */
    String content();
}
```
## bambda
### Bambda
```java
/*
 * 版权所有 (c) 2022-2024。PortSwigger Ltd. 保留所有权利。
 *
 * 此代码可用于扩展Burp Suite社区版和Burp Suite专业版的功能，
 * 前提是该使用不违反这些产品的许可条款。
 */

package burp.api.montoya.bambda;

/**
 * 提供与Bambda相关的功能访问接口。
 */
public interface Bambda
{
    /**
     * 导入Bambda脚本。
     * 如果库中已存在相同ID的脚本，将会被替换。
     *
     * @param script 要导入的Bambda脚本内容
     * @return 包含导入结果的{@link BambdaImportResult}对象
     */
    BambdaImportResult importBambda(String script);
}
```
### BambdaImportResult
```java
/*
 * 版权所有 (c) 2022-2024。PortSwigger Ltd. 保留所有权利。
 *
 * 此代码可用于扩展Burp Suite社区版和Burp Suite专业版的功能，
 * 前提是该使用不违反这些产品的许可条款。
 */

package burp.api.montoya.bambda;

import java.util.List;

/**
 * 表示Bambda脚本导入的结果。
 */
public interface BambdaImportResult
{
    /**
     * Bambda脚本导入状态枚举。
     */
    enum Status
    {
        /**
         * 成功导入且无错误
         */
        LOADED_WITHOUT_ERRORS,
        
        /**
         * 成功导入但存在错误
         */
        LOADED_WITH_ERRORS
    }

    /**
     * 获取Bambda脚本的导入状态。
     *
     * @return 导入状态枚举值
     */
    Status status();

    /**
     * 获取导入过程中的错误信息列表。
     *
     * @return 错误信息列表，如果没有错误则返回空列表
     */
    List<String> importErrors();
}
```
## burpsuite
### BurpSuite
```java
/*
 * 版权所有 (c) 2022-2023。PortSwigger Ltd. 保留所有权利。
 *
 * 此代码可用于扩展Burp Suite社区版和Burp Suite专业版的功能，
 * 前提是该使用不违反这些产品的许可条款。
 */

package burp.api.montoya.burpsuite;

import burp.api.montoya.core.Version;
import java.util.List;

/**
 * 提供访问Burp Suite应用程序相关功能的接口。
 */
public interface BurpSuite
{
    /**
     * 获取当前运行的Burp版本信息。
     * 扩展可根据当前版本支持的功能和API动态调整其行为。
     *
     * @return Burp的{@link Version}版本信息
     */
    Version version();

    /**
     * 以JSON格式导出当前项目级别的配置。
     * 此格式与通过Burp用户界面保存和加载的格式相同。
     * 要仅包含配置的特定部分，可选择性提供每个部分的路径，
     * 例如："project_options.connections"。
     * 如不提供路径，则将保存整个配置。
     *
     * @param paths 表示应包含的配置部分路径的字符串数组
     * @return 表示当前配置的JSON格式字符串
     */
    String exportProjectOptionsAsJson(String... paths);

    /**
     * 从提供的JSON字符串导入新的项目级别配置。
     * 此格式与通过Burp用户界面保存和加载的格式相同。
     * 可接受部分配置，未指定的设置将保持不变。
     * <p>
     * 输入中包含的任何用户级别配置选项将被忽略。
     *
     * @param json 包含新配置的JSON字符串
     */
    void importProjectOptionsFromJson(String json);

    /**
     * 以JSON格式导出当前用户级别的配置。
     * 此格式与通过Burp用户界面保存和加载的格式相同。
     * 要仅包含配置的特定部分，可选择性提供每个部分的路径，
     * 例如："user_options.connections"。
     * 如不提供路径，则将保存整个配置。
     *
     * @param paths 表示应包含的配置部分路径的字符串数组
     * @return 表示当前配置的JSON格式字符串
     */
    String exportUserOptionsAsJson(String... paths);

    /**
     * 从提供的JSON字符串导入新的用户级别配置。
     * 此格式与通过Burp用户界面保存和加载的格式相同。
     * 可接受部分配置，未指定的设置将保持不变。
     * <p>
     * 输入中包含的任何项目级别配置选项将被忽略。
     *
     * @param json 包含新配置的JSON字符串
     */
    void importUserOptionsFromJson(String json);

    /**
     * 获取启动时传递给Burp的命令行参数。
     *
     * @return 启动时传递给Burp的命令行参数列表
     */
    List<String> commandLineArguments();

    /**
     * 以编程方式关闭Burp。
     *
     * @param options 关闭选项，例如{@link ShutdownOptions#PROMPT_USER}将
     *                向用户显示对话框，允许他们确认或取消关闭操作。
     */
    void shutdown(ShutdownOptions... options);

    /**
     * 访问任务执行引擎的功能。
     *
     * @return 实现TaskExecutionEngine接口的对象，提供任务执行引擎功能
     */
    TaskExecutionEngine taskExecutionEngine();
}
```
### ShutdownOptions
```java
/*
 * 版权所有 (c) 2022-2023。PortSwigger Ltd. 保留所有权利。
 *
 * 此代码可用于扩展Burp Suite社区版和Burp Suite专业版的功能，
 * 前提是该使用不违反这些产品的许可条款。
 */

package burp.api.montoya.burpsuite;

/**
 * 关闭Burp Suite时可使用的选项枚举。
 * 用于{@link BurpSuite#shutdown(ShutdownOptions...)}方法。
 */
public enum ShutdownOptions
{
    /**
     * 向用户显示确认对话框，允许用户确认或取消关闭操作
     */
    PROMPT_USER
}
```
### TaskExecutionEngine
```java
/*
 * 版权所有 (c) 2022-2023。PortSwigger Ltd. 保留所有权利。
 *
 * 此代码可用于扩展Burp Suite社区版和Burp Suite专业版的功能，
 * 前提是该使用不违反这些产品的许可条款。
 */

package burp.api.montoya.burpsuite;

/**
 * 提供对任务执行引擎的访问接口。
 */
public interface TaskExecutionEngine
{
    /**
     * 任务执行引擎状态枚举。
     */
    enum TaskExecutionEngineState
    {
        /**
         * 运行状态
         */
        RUNNING,
        /**
         * 暂停状态
         */
        PAUSED
    }

    /**
     * 获取任务执行引擎的当前状态。
     *
     * @return 当前状态枚举值
     */
    TaskExecutionEngineState getState();

    /**
     * 设置任务执行引擎的状态。
     *
     * @param state 要设置的新状态
     */
    void setState(TaskExecutionEngineState state);
}
```
## collaborator
### Collaborator
```java
/*
 * 版权所有 (c) 2022-2023。PortSwigger Ltd. 保留所有权利。
 *
 * 此代码可用于扩展Burp Suite社区版和Burp Suite专业版的功能，
 * 前提是该使用不违反这些产品的许可条款。
 */

package burp.api.montoya.collaborator;

/**
 * [仅限专业版] 提供对Burp Collaborator功能的访问。
 */
public interface Collaborator
{
    /**
     * 创建新的Collaborator客户端，
     * 用于生成Collaborator payload并轮询服务器获取网络交互结果。
     *
     * @return 新的{@link CollaboratorClient}实例，可用于生成payload和获取交互记录
     */
    CollaboratorClient createClient();

    /**
     * 从之前的会话恢复{@link CollaboratorClient}。
     * 允许检索特定payload产生的交互记录。
     *
     * @param secretKey 用于恢复之前会话的密钥
     * @return 新的{@link CollaboratorClient}实例，可用于生成payload和获取交互记录
     */
    CollaboratorClient restoreClient(SecretKey secretKey);

    /**
     * 获取Burp默认的Collaborator payload生成器。
     * 生成的payload会关联到Collaborator标签页，
     * 交互结果会显示在生成payload时打开的Collaborator结果标签页中。
     *
     * @return 当前Burp默认的{@link CollaboratorPayloadGenerator}实例
     */
    CollaboratorPayloadGenerator defaultPayloadGenerator();
}
```
### CollaboratorClient
```java
/*
 * 版权所有 (c) 2022-2023。PortSwigger Ltd. 保留所有权利。
 *
 * 此代码可用于扩展Burp Suite社区版和Burp Suite专业版的功能，
 * 前提是该使用不违反这些产品的许可条款。
 */

package burp.api.montoya.collaborator;

import java.util.List;

/**
 * Burp Collaborator客户端接口，
 * 用于生成Collaborator payload并轮询服务器获取使用这些payload产生的网络交互。
 * 扩展可通过调用{@link Collaborator#createClient()}获取此接口的新实例。
 * <p>
 * 注意：每个Collaborator客户端都与创建时配置的Collaborator服务器绑定。
 * </p>
 */
public interface CollaboratorClient extends CollaboratorPayloadGenerator
{
    /**
     * 生成新的Collaborator payload。
     * 可通过选项参数指定payload生成方式。
     * 如未指定选项，生成的payload将包含服务器位置信息。
     *
     * @param options 可选的payload生成选项
     * @return 生成的payload对象
     * @throws IllegalStateException 如果Collaborator功能被禁用
     */
    @Override
    CollaboratorPayload generatePayload(PayloadOption... options);

    /**
     * 生成包含自定义数据的新Collaborator payload。
     * 自定义数据可从任何触发的{@link Interaction}中获取。
     * 可通过选项参数指定payload生成方式。
     * 如未指定选项，生成的payload将包含服务器位置信息。
     *
     * @param customData 要添加到payload的自定义数据(最大16字符，必须为字母数字)
     * @param options 可选的payload生成选项
     * @return 生成的payload对象
     * @throws IllegalStateException 如果Collaborator功能被禁用
     */
    CollaboratorPayload generatePayload(String customData, PayloadOption... options);

    /**
     * 获取由此客户端生成的所有payload产生的交互记录。
     *
     * @return 此客户端生成payload所产生的所有交互记录列表
     * @throws IllegalStateException 如果Collaborator功能被禁用
     */
    List<Interaction> getAllInteractions();

    /**
     * 获取由此客户端生成payload产生的、符合过滤条件的交互记录。
     *
     * @param filter 应用于每个交互记录的过滤器
     * @return 符合过滤条件的交互记录列表
     * @throws IllegalStateException 如果Collaborator功能被禁用
     */
    List<Interaction> getInteractions(InteractionFilter filter);

    /**
     * 获取与此客户端关联的Collaborator服务器详情。
     *
     * @return Collaborator服务器详情对象
     * @throws IllegalStateException 如果Collaborator功能被禁用
     */
    CollaboratorServer server();

    /**
     * 获取与此客户端上下文关联的密钥。
     * 该密钥可用于在需要时重新创建包含交互数据的客户端。
     *
     * @return 与此Collaborator客户端关联的{@link SecretKey}
     */
    SecretKey getSecretKey();
}
```
### CollaboratorPayload
```java
/*
 * 版权所有 (c) 2022-2023。PortSwigger Ltd. 保留所有权利。
 *
 * 此代码可用于扩展Burp Suite社区版和Burp Suite专业版的功能，
 * 前提是该使用不违反这些产品的许可条款。
 */

package burp.api.montoya.collaborator;

import java.util.Optional;

/**
 * Burp Collaborator payload接口，表示生成的Collaborator payload。
 */
public interface CollaboratorPayload
{
    /**
     * 获取payload的交互ID。
     *
     * @return payload的交互ID对象
     */
    InteractionId id();

    /**
     * 获取payload中的自定义数据。
     *
     * @return 包含自定义数据的Optional对象，若无自定义数据则为空
     */
    Optional<String> customData();

    /**
     * 获取payload引用的Collaborator服务器信息。
     * 如果payload生成时未包含服务器位置信息，则返回空的Optional。
     *
     * @return 包含服务器详情的Optional对象，若无服务器信息则为空
     */
    Optional<CollaboratorServer> server();

    /**
     * 获取payload的字符串表示形式。
     *
     * @return payload字符串
     */
    @Override
    String toString();
}
```
### CollaboratorPayloadGenerator
```java
/*
 * 版权所有 (c) 2022-2023。PortSwigger Ltd. 保留所有权利。
 *
 * 此代码可用于扩展Burp Suite社区版和Burp Suite专业版的功能，
 * 前提是该使用不违反这些产品的许可条款。
 */

package burp.api.montoya.collaborator;

/**
 * Burp Collaborator payload生成器接口，
 * 用于生成Burp Collaborator payload。
 */
public interface CollaboratorPayloadGenerator
{
    /**
     * 生成新的Collaborator payload。
     * 可通过选项参数指定payload生成方式。
     * 如未指定选项，生成的payload将包含服务器位置信息。
     *
     * @param options 可选的payload生成选项
     * @return 生成的payload对象
     * @throws IllegalStateException 如果Collaborator功能被禁用
     */
    CollaboratorPayload generatePayload(PayloadOption... options);
}
```
### CollaboratorServer
```java
/*
 * 版权所有 (c) 2022-2023。PortSwigger Ltd. 保留所有权利。
 *
 * 此代码可用于扩展Burp Suite社区版和Burp Suite专业版的功能，
 * 前提是该使用不违反这些产品的许可条款。
 */

package burp.api.montoya.collaborator;

/**
 * 提供与当前客户端关联的Collaborator服务器详细信息。
 */
public interface CollaboratorServer
{
    /**
     * 获取Collaborator服务器地址。
     *
     * @return Collaborator服务器的主机名或IP地址
     */
    String address();

    /**
     * 检查服务器地址是否为IP地址格式。
     *
     * @return 如果是IP地址返回{@code true}，否则返回{@code false}
     */
    boolean isLiteralAddress();
}
```
### DnsDetails
```java
/*
 * 版权所有 (c) 2022-2023。PortSwigger Ltd. 保留所有权利。
 *
 * 此代码可用于扩展Burp Suite社区版和Burp Suite专业版的功能，
 * 前提是该使用不违反这些产品的许可条款。
 */

package burp.api.montoya.collaborator;

import burp.api.montoya.core.ByteArray;

/**
 * 提供Burp Collaborator检测到的DNS交互信息。
 */
public interface DnsDetails
{
    /**
     * 获取DNS查询类型。
     *
     * @return 交互执行的DNS查询类型
     */
    DnsQueryType queryType();

    /**
     * 获取原始DNS查询数据。
     *
     * @return 发送到Collaborator服务器的原始DNS查询数据
     */
    ByteArray query();
}
```
### DnsQueryType
```java

```
### HttpDetails
```java
/*
 * 版权所有 (c) 2022-2023。PortSwigger Ltd. 保留所有权利。
 *
 * 此代码可用于扩展Burp Suite社区版和Burp Suite专业版的功能，
 * 前提是该使用不违反这些产品的许可条款。
 */

package burp.api.montoya.collaborator;

/**
 * 域名系统(DNS)查询类型枚举
 */
public enum DnsQueryType
{
    /**
     * IPv4地址记录
     */
    A,
    /**
     * IPv6地址记录 
     */
    AAAA,
    /**
     * 所有缓存记录
     */
    ALL,
    /**
     * 证书颁发机构授权
     */
    CAA,
    /**
     * 规范名称记录
     */
    CNAME,
    /**
     * DNS密钥记录
     */
    DNSKEY,
    /**
     * 委派签名记录
     */
    DS,
    /**
     * 主机信息记录
     */
    HINFO,
    /**
     * HTTPS绑定记录
     */
    HTTPS,
    /**
     * 邮件交换记录
     */
    MX,
    /**
     * 命名权威指针记录
     */
    NAPTR,
    /**
     * 名称服务器记录
     */
    NS,
    /**
     * 指针资源记录
     */
    PTR,
    /**
     * 起始授权记录
     */
    SOA,
    /**
     * 服务定位记录
     */
    SRV,
    /**
     * 文本记录
     */
    TXT,
    /**
     * 未知/未映射/已废弃记录
     */
    UNKNOWN
}
```
### HttpDetails
```java
/*
 * 版权所有 (c) 2022-2023。PortSwigger Ltd. 保留所有权利。
 *
 * 此代码可用于扩展Burp Suite社区版和Burp Suite专业版的功能，
 * 前提是该使用不违反这些产品的许可条款。
 */

package burp.api.montoya.collaborator;

import burp.api.montoya.http.HttpProtocol;
import burp.api.montoya.http.message.HttpRequestResponse;

/**
 * 提供Burp Collaborator检测到的HTTP交互信息。
 */
public interface HttpDetails
{
    /**
     * 获取HTTP协议版本。
     *
     * @return 交互使用的HTTP协议
     */
    HttpProtocol protocol();

    /**
     * 获取HTTP请求和响应。
     *
     * @return 发送到Collaborator服务器的HTTP请求及其响应
     */
    HttpRequestResponse requestResponse();
}
```
### Interaction
```java
/*
 * 版权所有 (c) 2022-2023。PortSwigger Ltd. 保留所有权利。
 *
 * 此代码可用于扩展Burp Suite社区版和Burp Suite专业版的功能，
 * 前提是该使用不违反这些产品的许可条款。
 */

package burp.api.montoya.collaborator;

import java.net.InetAddress;
import java.time.ZonedDateTime;
import java.util.Optional;

/**
 * 提供与Burp Collaborator服务器交互的详细信息。
 */
public interface Interaction
{
    /**
     * 获取交互的唯一标识符。
     *
     * @return 交互ID对象
     */
    InteractionId id();

    /**
     * 获取交互类型。
     *
     * @return 交互类型枚举值
     */
    InteractionType type();

    /**
     * 获取交互发生的时间戳。
     *
     * @return 包含时区信息的交互时间
     */
    ZonedDateTime timeStamp();

    /**
     * 获取发起交互的客户端IP地址。
     *
     * @return 客户端IP地址对象
     */
    InetAddress clientIp();

    /**
     * 获取发起交互的客户端端口号。
     *
     * @return 客户端端口号
     */
    int clientPort();

    /**
     * 获取DNS交互的详细信息(如果存在)。
     *
     * @return 包含DNS详情的Optional对象，若非DNS交互则为空
     */
    Optional<DnsDetails> dnsDetails();

    /**
     * 获取HTTP交互的详细信息(如果存在)。
     *
     * @return 包含HTTP详情的Optional对象，若非HTTP交互则为空
     */
    Optional<HttpDetails> httpDetails();

    /**
     * 获取SMTP交互的详细信息(如果存在)。
     *
     * @return 包含SMTP详情的Optional对象，若非SMTP交互则为空
     */
    Optional<SmtpDetails> smtpDetails();

    /**
     * 获取payload中的自定义数据(如果存在)。
     *
     * @return 包含自定义数据的Optional对象，若无自定义数据则为空
     */
    Optional<String> customData();
}
```
### InteractionFilter
```java
/*
 * 版权所有 (c) 2022-2023。PortSwigger Ltd. 保留所有权利。
 *
 * 此代码可用于扩展Burp Suite社区版和Burp Suite专业版的功能，
 * 前提是该使用不违反这些产品的许可条款。
 */

package burp.api.montoya.collaborator;

import static burp.api.montoya.internal.ObjectFactoryLocator.FACTORY;

/**
 * 提供从Burp Collaborator服务器检索交互时的过滤机制。
 * 提供了基于交互ID和payload创建过滤器的辅助方法。
 */
public interface InteractionFilter
{
    /**
     * 对从Collaborator服务器检索的每个交互调用此方法，
     * 以确定该交互是否应包含在返回的交互列表中。
     *
     * @param server      接收交互的Collaborator服务器
     * @param interaction 交互详细信息
     * @return 如果应包含该交互返回{@code true}，否则返回{@code false}
     */
    boolean matches(CollaboratorServer server, Interaction interaction);

    /**
     * 创建匹配指定交互ID的过滤器。
     *
     * @param id 要匹配的交互ID
     * @return 匹配指定ID的交互过滤器
     */
    static InteractionFilter interactionIdFilter(String id)
    {
        return FACTORY.interactionIdFilter(id);
    }

    /**
     * 创建匹配指定payload的过滤器。
     *
     * @param payload 要匹配的payload
     * @return 匹配指定payload的交互过滤器
     */
    static InteractionFilter interactionPayloadFilter(String payload)
    {
        return FACTORY.interactionPayloadFilter(payload);
    }
}
```
### InteractionId
```java
/*
 * 版权所有 (c) 2022-2023。PortSwigger Ltd. 保留所有权利。
 *
 * 此代码可用于扩展Burp Suite社区版和Burp Suite专业版的功能，
 * 前提是该使用不违反这些产品的许可条款。
 */

package burp.api.montoya.collaborator;

/**
 * 表示Burp Collaborator交互的唯一标识符。
 */
public interface InteractionId
{
    /**
     * 获取交互标识符的字符串表示形式。
     *
     * @return 交互ID字符串
     */
    @Override
    String toString();
}
```
### InteractionType
```java
/*
 * 版权所有 (c) 2022-2023。PortSwigger Ltd. 保留所有权利。
 *
 * 此代码可用于扩展Burp Suite社区版和Burp Suite专业版的功能，
 * 前提是该使用不违反这些产品的许可条款。
 */

package burp.api.montoya.collaborator;

/**
 * Burp Collaborator支持的交互协议类型枚举。
 */
public enum InteractionType
{
    /**
     * 域名系统协议(DNS)
     */
    DNS,
    /**
     * 超文本传输协议(HTTP)
     */
    HTTP,
    /**
     * 简单邮件传输协议(SMTP)
     */
    SMTP
}
```
### PayloadOption
```java
/*
 * 版权所有 (c) 2023。PortSwigger Ltd. 保留所有权利。
 *
 * 此代码可用于扩展Burp Suite社区版和Burp Suite专业版的功能，
 * 前提是该使用不违反这些产品的许可条款。
 */

package burp.api.montoya.collaborator;

/**
 * 生成Burp Collaborator payload时可指定的选项枚举。
 */
public enum PayloadOption
{
    /**
     * 生成不包含服务器位置信息的payload
     */
    WITHOUT_SERVER_LOCATION
}
```
### SecretKey
```java
/*
 * 版权所有 (c) 2023。PortSwigger Ltd. 保留所有权利。
 *
 * 此代码可用于扩展Burp Suite社区版和Burp Suite专业版的功能，
 * 前提是该使用不违反这些产品的许可条款。
 */

package burp.api.montoya.collaborator;

import static burp.api.montoya.internal.ObjectFactoryLocator.FACTORY;

/**
 * 表示与{@link CollaboratorClient}关联的密钥。
 */
public interface SecretKey
{
    /**
     * 获取密钥的字符串表示形式。
     *
     * @return Base64编码的密钥字符串
     */
    @Override
    String toString();

    /**
     * 创建{@link SecretKey}实例，用于通过{@link Collaborator#restoreClient(SecretKey)}方法
     * 恢复之前创建的{@link CollaboratorClient}。
     *
     * @param encodedKey Base64编码的原始密钥字符串
     * @return 包装了指定密钥的{@link SecretKey}实例
     */
    static SecretKey secretKey(String encodedKey)
    {
        return FACTORY.secretKey(encodedKey);
    }
}
```
### SmtpDetails
```java
/*
 * 版权所有 (c) 2022-2023。PortSwigger Ltd. 保留所有权利。
 *
 * 此代码可用于扩展Burp Suite社区版和Burp Suite专业版的功能，
 * 前提是该使用不违反这些产品的许可条款。
 */

package burp.api.montoya.collaborator;

/**
 * 提供Burp Collaborator检测到的SMTP交互详细信息。
 */
public interface SmtpDetails
{
    /**
     * 获取SMTP交互使用的协议信息。
     *
     * @return SMTP协议信息对象
     */
    SmtpProtocol protocol();

    /**
     * 获取SMTP会话内容。
     *
     * @return 客户端与Collaborator服务器之间的SMTP会话内容字符串
     */
    String conversation();
}
```
### SmtpProtocol
```java
/*
 * 版权所有 (c) 2022-2023。PortSwigger Ltd. 保留所有权利。
 *
 * 此代码可用于扩展Burp Suite社区版和Burp Suite专业版的功能，
 * 前提是该使用不违反这些产品的许可条款。
 */

package burp.api.montoya.collaborator;

/**
 * 简单邮件传输协议(SMTP)类型枚举。
 */
public enum SmtpProtocol
{
    /**
     * 简单邮件传输协议(SMTP)
     */
    SMTP,
    /**
     * 安全简单邮件传输协议(SMTPS)
     */
    SMTPS
}
```
## comparer
### Comparer
```java
/*
 * 版权所有 (c) 2022-2023。PortSwigger Ltd. 保留所有权利。
 *
 * 此代码可用于扩展Burp Suite社区版和Burp Suite专业版的功能，
 * 前提是该使用不违反这些产品的许可条款。
 */

package burp.api.montoya.comparer;

import burp.api.montoya.core.ByteArray;

/**
 * 提供对比较工具功能的访问接口。
 */
public interface Comparer
{
    /**
     * 将数据发送到比较工具进行比对分析。
     *
     * @param data 要发送到比较工具的字节数组数据(可变参数)
     */
    void sendToComparer(ByteArray... data);
}
```
## core
### Annotations
```java
/*
 * 版权所有 (c) 2022-2023。PortSwigger Ltd. 保留所有权利。
 *
 * 此代码可用于扩展Burp Suite社区版和Burp Suite专业版的功能，
 * 前提是该使用不违反这些产品的许可条款。
 */

package burp.api.montoya.core;

import static burp.api.montoya.internal.ObjectFactoryLocator.FACTORY;

/**
 * 存储在Burp Suite中与请求和响应关联的注解信息。
 */
public interface Annotations
{
    /**
     * @return 返回注解笔记内容
     */
    String notes();

    /**
     * @return 如果该HTTP请求和响应存在笔记则返回true
     */
    boolean hasNotes();

    /**
     * @return 如果该HTTP请求和响应存在高亮颜色则返回true
     */
    boolean hasHighlightColor();

    /**
     * 设置（修改）当前注解的笔记内容
     *
     * @param notes 要设置的笔记内容
     */
    void setNotes(String notes);

    /**
     * @return 返回高亮颜色
     */
    HighlightColor highlightColor();

    /**
     * 设置（修改）当前注解的高亮颜色
     *
     * @param highlightColor 要设置的高亮颜色
     */
    void setHighlightColor(HighlightColor highlightColor);

    /**
     * 创建带有新笔记的注解副本
     *
     * @param notes 新笔记内容
     *
     * @return 新的注解实例
     */
    Annotations withNotes(String notes);

    /**
     * 创建带有新高亮颜色的注解副本
     *
     * @param highlightColor 新高亮颜色
     *
     * @return 新的注解实例
     */
    Annotations withHighlightColor(HighlightColor highlightColor);

    /**
     * 创建一个新的空注解
     *
     * @return 注解实例
     */
    static Annotations annotations()
    {
        return FACTORY.annotations();
    }

    /**
     * 创建带有笔记的新注解
     *
     * @param notes 注解笔记内容
     *
     * @return 注解实例
     */
    static Annotations annotations(String notes)
    {
        return FACTORY.annotations(notes);
    }

    /**
     * 创建带有高亮颜色的新注解
     *
     * @param highlightColor 注解高亮颜色
     *
     * @return 注解实例
     */
    static Annotations annotations(HighlightColor highlightColor)
    {
        return FACTORY.annotations(highlightColor);
    }

    /**
     * 创建同时带有笔记和高亮颜色的新注解
     *
     * @param notes        注解笔记内容
     * @param highlightColor 注解高亮颜色
     *
     * @return 注解实例
     */
    static Annotations annotations(String notes, HighlightColor highlightColor)
    {
        return FACTORY.annotations(notes, highlightColor);
    }
}
```
###  BurpSuiteEdition
```java
/*
 * 版权所有 (c) 2023。PortSwigger Ltd. 保留所有权利。
 *
 * 此代码可用于扩展Burp Suite社区版和Burp Suite专业版的功能，
 * 前提是该使用不违反这些产品的许可条款。
 */

package burp.api.montoya.core;

/**
 * Burp Suite的各个版本枚举。
 */
public enum BurpSuiteEdition
{
    /**
     * Burp Suite专业版
     */
    PROFESSIONAL("Professional"),
    /**
     * Burp Suite社区版
     */
    COMMUNITY_EDITION("Community Edition"),
    /**
     * Burp Suite企业版
     */
    ENTERPRISE_EDITION("Enterprise Edition");

    private final String displayName;

    BurpSuiteEdition(String displayName)
    {
        this.displayName = displayName;
    }

    /**
     * @return 返回该Burp Suite版本的显示名称
     */
    public String displayName()
    {
        return displayName;
    }
}
```
###  ByteArray
```java
/*
 * 版权所有 (c) 2023。PortSwigger Ltd. 保留所有权利。
 *
 * 此代码可用于扩展Burp Suite社区版和Burp Suite专业版的功能，
 * 前提是该使用不违反这些产品的许可条款。
 */

package burp.api.montoya.core;

import java.util.regex.Pattern;

import static burp.api.montoya.internal.ObjectFactoryLocator.FACTORY;

/**
 * 提供多种查询和操作字节数组方法的Burp ByteArray接口。
 */
public interface ByteArray extends Iterable<Byte>
{
    /**
     * 获取指定索引处的字节。
     *
     * @param index 要获取的字节索引
     * @return 索引处的字节值
     */
    byte getByte(int index);

    /**
     * 设置指定索引处的字节值。
     *
     * @param index 要设置的字节索引
     * @param value 要设置的字节值
     */
    void setByte(int index, byte value);

    /**
     * 设置指定索引处的字节值（将整型值窄化为字节）。
     *
     * @param index 要设置的字节索引
     * @param value 要设置的整型值（将被窄化为字节）
     */
    void setByte(int index, int value);

    /**
     * 从指定索引开始设置多个字节。
     *
     * @param index 起始字节索引
     * @param data 要设置的字节数组或字节序列
     */
    void setBytes(int index, byte... data);

    /**
     * 从指定索引开始设置多个字节（将整型值窄化为字节）。
     *
     * @param index 起始字节索引
     * @param data 要设置的整型数组或整型序列（将被窄化为字节）
     */
    void setBytes(int index, int... data);

    /**
     * 从指定索引开始设置多个字节。
     *
     * @param index 起始字节索引
     * @param byteArray 包含要设置字节的ByteArray对象
     */
    void setBytes(int index, ByteArray byteArray);

    /**
     * 获取ByteArray的长度。
     *
     * @return ByteArray的字节长度
     */
    int length();

    /**
     * 获取所有字节的副本。
     *
     * @return 字节数组副本
     */
    byte[] getBytes();

    /**
     * 获取指定范围内的子数组。
     *
     * @param startIndexInclusive 起始索引（包含）
     * @param endIndexExclusive 结束索引（不包含）
     * @return 包含指定范围内字节的新ByteArray
     */
    ByteArray subArray(int startIndexInclusive, int endIndexExclusive);

    /**
     * 获取指定范围内的子数组。
     *
     * @param range 要获取的字节范围
     * @return 包含指定范围内字节的新ByteArray
     */
    ByteArray subArray(Range range);

    /**
     * 创建ByteArray的副本。
     *
     * @return 包含相同字节的新ByteArray
     */
    ByteArray copy();

    /**
     * 创建ByteArray的临时文件副本。<br>
     * 此方法用于将ByteArray对象保存到临时文件中，
     * 使其不再保留在内存中。扩展可以使用此方法将
     * ByteArray对象转换为适合长期使用的形式。
     *
     * @return 存储在临时文件中的新ByteArray实例
     */
    ByteArray copyToTempFile();

    /**
     * 在ByteArray中搜索指定术语的首次出现位置。
     * 其工作方式类似于Java原生方法{@link String#indexOf(String)}。
     *
     * @param searchTerm 要搜索的值
     * @return 首次出现位置的偏移量，未找到则返回-1
     */
    int indexOf(ByteArray searchTerm);

    /**
     * 在ByteArray中搜索指定字符串的首次出现位置。
     *
     * @param searchTerm 要搜索的字符串
     * @return 首次出现位置的偏移量，未找到则返回-1
     */
    int indexOf(String searchTerm);

    /**
     * 在ByteArray中搜索指定术语的首次出现位置（可选区分大小写）。
     *
     * @param searchTerm 要搜索的值
     * @param caseSensitive 是否区分大小写
     * @return 首次出现位置的偏移量，未找到则返回-1
     */
    int indexOf(ByteArray searchTerm, boolean caseSensitive);

    /**
     * 在ByteArray中搜索指定字符串的首次出现位置（可选区分大小写）。
     *
     * @param searchTerm 要搜索的字符串
     * @param caseSensitive 是否区分大小写
     * @return 首次出现位置的偏移量，未找到则返回-1
     */
    int indexOf(String searchTerm, boolean caseSensitive);

    /**
     * 在指定范围内搜索术语的首次出现位置（可选区分大小写）。
     *
     * @param searchTerm 要搜索的值
     * @param caseSensitive 是否区分大小写
     * @param startIndexInclusive 起始索引（包含）
     * @param endIndexExclusive 结束索引（不包含）
     * @return 首次出现位置的偏移量，未找到则返回-1
     */
    int indexOf(ByteArray searchTerm, boolean caseSensitive, int startIndexInclusive, int endIndexExclusive);

    /**
     * 在指定范围内搜索字符串的首次出现位置（可选区分大小写）。
     *
     * @param searchTerm 要搜索的字符串
     * @param caseSensitive 是否区分大小写
     * @param startIndexInclusive 起始索引（包含）
     * @param endIndexExclusive 结束索引（不包含）
     * @return 首次出现位置的偏移量，未找到则返回-1
     */
    int indexOf(String searchTerm, boolean caseSensitive, int startIndexInclusive, int endIndexExclusive);

    /**
     * 搜索正则表达式的首次匹配位置。
     *
     * @param pattern 要匹配的正则表达式
     * @return 首次匹配位置的偏移量，未找到则返回-1
     */
    int indexOf(Pattern pattern);

    /**
     * 在指定范围内搜索正则表达式的首次匹配位置。
     *
     * @param pattern 要匹配的正则表达式
     * @param startIndexInclusive 起始索引（包含）
     * @param endIndexExclusive 结束索引（不包含）
     * @return 首次匹配位置的偏移量，未找到则返回-1
     */
    int indexOf(Pattern pattern, int startIndexInclusive, int endIndexExclusive);

    /**
     * 统计ByteArray中指定术语的匹配次数。
     *
     * @param searchTerm 要搜索的值
     * @return 匹配次数
     */
    int countMatches(ByteArray searchTerm);

    /**
     * 统计ByteArray中指定字符串的匹配次数。
     *
     * @param searchTerm 要搜索的字符串
     * @return 匹配次数
     */
    int countMatches(String searchTerm);

    /**
     * 统计ByteArray中指定术语的匹配次数（可选区分大小写）。
     *
     * @param searchTerm 要搜索的值
     * @param caseSensitive 是否区分大小写
     * @return 匹配次数
     */
    int countMatches(ByteArray searchTerm, boolean caseSensitive);

    /**
     * 统计ByteArray中指定字符串的匹配次数（可选区分大小写）。
     *
     * @param searchTerm 要搜索的字符串
     * @param caseSensitive 是否区分大小写
     * @return 匹配次数
     */
    int countMatches(String searchTerm, boolean caseSensitive);

    /**
     * 在指定范围内统计术语的匹配次数（可选区分大小写）。
     *
     * @param searchTerm 要搜索的值
     * @param caseSensitive 是否区分大小写
     * @param startIndexInclusive 起始索引（包含）
     * @param endIndexExclusive 结束索引（不包含）
     * @return 匹配次数
     */
    int countMatches(ByteArray searchTerm, boolean caseSensitive, int startIndexInclusive, int endIndexExclusive);

    /**
     * 在指定范围内统计字符串的匹配次数（可选区分大小写）。
     *
     * @param searchTerm 要搜索的字符串
     * @param caseSensitive 是否区分大小写
     * @param startIndexInclusive 起始索引（包含）
     * @param endIndexExclusive 结束索引（不包含）
     * @return 匹配次数
     */
    int countMatches(String searchTerm, boolean caseSensitive, int startIndexInclusive, int endIndexExclusive);

    /**
     * 统计正则表达式的匹配次数。
     *
     * @param pattern 要匹配的正则表达式
     * @return 匹配次数
     */
    int countMatches(Pattern pattern);

    /**
     * 在指定范围内统计正则表达式的匹配次数。
     *
     * @param pattern 要匹配的正则表达式
     * @param startIndexInclusive 起始索引（包含）
     * @param endIndexExclusive 结束索引（不包含）
     * @return 匹配次数
     */
    int countMatches(Pattern pattern, int startIndexInclusive, int endIndexExclusive);

    /**
     * 使用Burp Suite指定的编码将ByteArray转换为字符串。
     *
     * @return 转换后的字符串
     */
    @Override
    String toString();

    /**
     * 创建追加了指定字节的新ByteArray副本。
     *
     * @param data 要追加的字节数组或字节序列
     */
    ByteArray withAppended(byte... data);

    /**
     * 创建追加了指定整型值（窄化为字节）的新ByteArray副本。
     *
     * @param data 要追加的整型数组或整型序列
     */
    ByteArray withAppended(int... data);

    /**
     * 创建追加了指定文本的新ByteArray副本。
     *
     * @param text 要追加的字符串
     */
    ByteArray withAppended(String text);

    /**
     * 创建追加了指定ByteArray的新ByteArray副本。
     *
     * @param byteArray 要追加的ByteArray
     */
    ByteArray withAppended(ByteArray byteArray);

    /**
     * 创建指定长度的新ByteArray。<br>
     *
     * @param length 数组长度
     * @return 指定长度的新ByteArray
     */
    static ByteArray byteArrayOfLength(int length)
    {
        return FACTORY.byteArrayOfLength(length);
    }

    /**
     * 创建包含指定字节数据的新ByteArray。<br>
     *
     * @param data 要包装的字节数组或字节序列
     * @return 包装指定字节数组的新ByteArray
     */
    static ByteArray byteArray(byte... data)
    {
        return FACTORY.byteArray(data);
    }

    /**
     * 创建包含指定整型值（窄化为字节）的新ByteArray。<br>
     *
     * @param data 要包装的整型数组或整型序列
     * @return 包装指定数据的新ByteArray
     */
    static ByteArray byteArray(int... data)
    {
        return FACTORY.byteArray(data);
    }

    /**
     * 从指定文本创建新ByteArray（使用Burp Suite指定的编码）。<br>
     *
     * @param text 要转换为字节数组的文本
     * @return 包含文本字节的新ByteArray
     */
    static ByteArray byteArray(String text)
    {
        return FACTORY.byteArray(text);
    }
}
```
###  HighlightColor
```java
/*
 * Copyright (c) 2022-2023. PortSwigger Ltd. All rights reserved.
 *
 * 此代码可用于扩展Burp Suite社区版和Burp Suite专业版的功能，
 * 前提是该使用不违反这些产品的许可条款。
 */

package burp.api.montoya.core;

import static burp.api.montoya.internal.ObjectFactoryLocator.FACTORY;

/**
 * 可用于在Burp Suite中进行高亮显示的颜色枚举
 */
public enum HighlightColor
{
    NONE("None"),         // 无高亮
    RED("Red"),           // 红色
    ORANGE("Orange"),     // 橙色
    YELLOW("Yellow"),     // 黄色
    GREEN("Green"),       // 绿色
    CYAN("Cyan"),         // 青色
    BLUE("Blue"),         // 蓝色
    PINK("Pink"),         // 粉色
    MAGENTA("Magenta"),   // 洋红色
    GRAY("Gray");         // 灰色

    private final String displayName;  // 颜色显示名称

    HighlightColor(String displayName)
    {
        this.displayName = displayName;
    }

    /**
     * 获取颜色的显示名称
     * 
     * @return 颜色的显示名称
     */
    public String displayName()
    {
        return displayName;
    }

    /**
     * 根据显示名称字符串创建HighlightColor实例
     *
     * @param colorName 颜色的显示名称
     *
     * @return 对应的高亮颜色实例
     */
    public static HighlightColor highlightColor(String colorName)
    {
        return FACTORY.highlightColor(colorName);
    }
}
```
### Marker
```java
/*
 * 版权所有 (c) 2022-2023。PortSwigger Ltd. 保留所有权利。
 *
 * 此代码可用于扩展Burp Suite社区版和Burp Suite专业版的功能，
 * 前提是该使用不违反这些产品的许可条款。
 */

package burp.api.montoya.core;

import static burp.api.montoya.internal.ObjectFactoryLocator.FACTORY;

/**
 * 标记接口，表示请求和响应中包含有趣数据的范围。
 */
public interface Marker
{
    /**
     * 获取标记的范围。
     *
     * @return 标记的范围对象
     */
    Range range();

    /**
     * 使用指定范围创建一个标记对象。
     *
     * @param range 标记的范围
     *
     * @return 包含指定范围的标记对象
     */
    static Marker marker(Range range)
    {
        return FACTORY.marker(range);
    }

    /**
     * 使用起始和结束索引创建一个标记对象。
     *
     * @param startIndexInclusive 范围的起始索引（包含该值）
     * @param endIndexExclusive   范围的结束索引（不包含该值）
     *
     * @return 包含指定范围的标记对象
     */
    static Marker marker(int startIndexInclusive, int endIndexExclusive)
    {
        return FACTORY.marker(startIndexInclusive, endIndexExclusive);
    }
}
```
### Range
```java
/*
 * 版权所有 (c) 2022-2023。PortSwigger Ltd. 保留所有权利。
 *
 * 此代码可用于扩展Burp Suite社区版和Burp Suite专业版的功能，
 * 前提是该使用不违反这些产品的许可条款。
 */

package burp.api.montoya.core;

import static burp.api.montoya.internal.ObjectFactoryLocator.FACTORY;

/**
 * 表示两个整数值之间的范围，该范围包含起始值但不包含结束值。
 * 这是一个左闭右开区间 [start, end)。
 */
public interface Range
{
    /**
     * 获取范围的起始索引（包含该值）。
     *
     * @return 包含性的起始索引值
     */
    int startIndexInclusive();

    /**
     * 获取范围的结束索引（不包含该值）。
     *
     * @return 排除性的结束索引值
     */
    int endIndexExclusive();

    /**
     * 检查指定索引是否在范围内。
     *
     * @param index 要检查的索引值
     * @return 如果索引在范围内返回true，否则返回false
     */
    boolean contains(int index);

    /**
     * 使用起始和结束索引创建一个范围对象。
     *
     * @param startIndexInclusive 范围的起始索引（包含该值）
     * @param endIndexExclusive   范围的结束索引（不包含该值）
     * @return 新创建的范围对象
     */
    static Range range(int startIndexInclusive, int endIndexExclusive)
    {
        return FACTORY.range(startIndexInclusive, endIndexExclusive);
    }
}
```
### Registration
```java
/*
 * 版权所有 (c) 2022-2023。PortSwigger Ltd. 保留所有权利。
 *
 * 此代码可用于扩展Burp Suite社区版和Burp Suite专业版的功能，
 * 前提是该使用不违反这些产品的许可条款。
 */

package burp.api.montoya.core;

/**
 * 表示扩展在Burp Suite中注册对象时返回的注册凭证。
 * 该接口提供对注册状态的管理能力。
 */
public interface Registration
{
    /**
     * 检查当前注册对象是否仍处于有效注册状态。
     *
     * @return 如果对象仍处于注册状态返回true，否则返回false
     */
    boolean isRegistered();

    /**
     * 注销当前注册的对象。
     * 调用此方法后，相关注册将被移除，
     * 且isRegistered()将返回false。
     */
    void deregister();
}
```
### Task
```java
/*
 * 版权所有 (c) 2022-2023。PortSwigger Ltd. 保留所有权利。
 *
 * 此代码可用于扩展Burp Suite社区版和Burp Suite专业版的功能，
 * 前提是该使用不违反这些产品的许可条款。
 */

package burp.api.montoya.core;

/**
 * 表示Burp Suite仪表板中的任务项。
 * 该接口提供了对任务的基本操作能力。
 */
public interface Task
{
    /**
     * 删除当前任务。
     * 调用此方法将从仪表板中永久移除该任务项。
     */
    void delete();

    /**
     * 获取任务的当前状态信息。
     *
     * @return 返回描述任务当前状态的文本信息
     */
    String statusMessage();
}
```
### ToolSource
```java

/*
 * 版权所有 (c) 2022-2023。PortSwigger Ltd. 保留所有权利。
 *
 * 此代码可用于扩展 Burp Suite Community Edition 和 Burp Suite Professional 的功能，
 * 前提是该使用不违反这些产品的许可条款。
 */

package burp.api.montoya.core;

/**
 * 作为对象来源的工具接口。
 * 该接口用于标识 Burp Suite 中产生某个对象的工具来源。
 */
public interface ToolSource
{
    /**
     * 获取工具类型。
     *
     * @return 返回工具类型枚举值 {@link ToolType}。
     */
    ToolType toolType();

    /**
     * 判断当前工具来源是否来自指定的工具类型之一。
     *
     * @param toolType 要检查的工具类型可变参数，可传入多个 {@link ToolType}。
     *
     * @return 如果当前工具来源是任何一个指定的工具类型，则返回 {@code true}；
     *         否则返回 {@code false}。
     */
    boolean isFromTool(ToolType... toolType);
}
```
### ToolType
```java
/*
 * Copyright (c) 2022-2023. PortSwigger Ltd. All rights reserved.
 *
 * 此代码可用于扩展Burp Suite Community Edition和Burp Suite Professional的功能，
 * 前提是该使用不违反这些产品的许可条款。
 */

package burp.api.montoya.core;

/**
 * Burp Suite中的工具枚举类型。
 * Enum representing tools in Burp Suite.
 */
public enum ToolType
{
    SUITE("Suite"),                  // 套件(整个Burp Suite)
    TARGET("Target"),                // 目标工具
    PROXY("Proxy"),                  // 代理工具
    SCANNER("Scanner"),              // 扫描器工具
    INTRUDER("Intruder"),            // 入侵者工具
    REPEATER("Repeater"),            // 重放器工具
    LOGGER("Logger"),                // 日志记录器工具
    SEQUENCER("Sequencer"),          // 序列器工具
    DECODER("Decoder"),              // 解码器工具
    COMPARER("Comparer"),            // 比较器工具
    EXTENSIONS("Extensions"),        // 扩展工具
    RECORDED_LOGIN_REPLAYER("Recorded login replayer"),  // 记录登录重放工具
    ORGANIZER("Organizer"),          // 组织器工具
    BURP_AI("Burp AI");              // Burp AI工具

    private final String toolName;    // 工具名称

    /**
     * 构造函数
     * @param toolName 工具名称
     */
    ToolType(String toolName)
    {
        this.toolName = toolName;
    }

    /**
     * 获取工具名称
     * @return 工具名称字符串
     */
    public String toolName()
    {
        return toolName;
    }

    /**
     * 重写toString方法
     * @return 工具名称字符串
     */
    @Override
    public String toString()
    {
        return toolName;
    }
}
```
### Version
```java
/*
 * Copyright (c) 2022-2023. PortSwigger Ltd. All rights reserved.
 *
 * 此代码可用于扩展Burp Suite Community Edition和Burp Suite Professional的功能，
 * 前提是该使用不违反这些产品的许可条款。
 */

package burp.api.montoya.core;

/**
 * 产品版本信息接口。<br>
 * 例如: "Burp Suite Professional 2022.8.1-9320"
 */
public interface Version
{
    /**
     * 获取产品名称 (例如: "Burp Suite Professional")。
     *
     * @return 产品名称字符串
     */
    String name();

    /**
     * 获取主版本号 (例如: "2022.8")。
     *
     * @return 主版本号字符串
     * @deprecated 已弃用，建议使用 {@link #toString()} 或 {@link #buildNumber()} 替代
     */
    @Deprecated(forRemoval = true)
    String major();

    /**
     * 获取次版本号 (例如: "1")。
     *
     * @return 次版本号字符串
     * @deprecated 已弃用，建议使用 {@link #toString()} 或 {@link #buildNumber()} 替代
     */
    @Deprecated(forRemoval = true)
    String minor();

    /**
     * 获取构建号 (例如: "9320")。
     *
     * @return 构建号字符串
     * @deprecated 已弃用，建议使用 {@link #toString()} 或 {@link #buildNumber()} 替代
     */
    @Deprecated(forRemoval = true)
    String build();

    /**
     * 获取Burp Suite的构建号(长整型)。<br>
     * 可用于判断与不同版本Burp Suite的兼容性。<br>
     * 注意：不要尝试解析此数字，因为其格式可能会变化。
     *
     * @return 构建号(长整型)
     */
    long buildNumber();

    /**
     * 获取Burp Suite的版本类型(社区版/专业版)。
     *
     * @return BurpSuiteEdition枚举值，表示版本类型
     */
    BurpSuiteEdition edition();

    /**
     * 获取人类可读的版本字符串。<br>
     * 注意：不要尝试解析此字符串，因为其格式可能会变化。<br>
     * 另请参阅: {@link #buildNumber()}。
     *
     * @return 可读的版本字符串
     */
    @Override
    String toString();
}
```
## decoder
###  Decoder
```java
/*
 * Copyright (c) 2022-2023. PortSwigger Ltd. 保留所有权利。
 *
 * 此代码可用于扩展 Burp Suite Community Edition 和 Burp Suite Professional 的功能，
 * 前提是该使用不违反这些产品的许可条款。
 */

package burp.api.montoya.decoder;

import burp.api.montoya.core.ByteArray;

/**
 * 提供对解码器(Decoder)工具功能的访问接口。
 * 
 * <p>该接口允许将数据发送到 Burp Suite 的解码器工具进行处理。</p>
 */
public interface Decoder
{
    /**
     * 将数据发送到解码器工具。
     *
     * @param data 要发送到解码器的数据，使用 ByteArray 类型表示
     * 
     * <p>示例用法：</p>
     * <pre>
     * byte[] rawData = ...;
     * ByteArray data = ByteArray.byteArray(rawData);
     * decoder.sendToDecoder(data);
     * </pre>
     * 
     * <p>注意：此操作会将数据发送到解码器工具的输入面板，但不会自动执行解码操作。</p>
     */
    void sendToDecoder(ByteArray data);
}
```
## extension
### Extension
```java 
/*
 * Copyright (c) 2022-2023. PortSwigger Ltd. 保留所有权利。
 *
 * 此代码可用于扩展Burp Suite Community Edition和Burp Suite Professional的功能，
 * 前提是该使用不违反这些产品的许可条款。
 */

package burp.api.montoya.extension;

import burp.api.montoya.core.Registration;

/**
 * 提供与扩展(Extension)相关的功能接口。
 * <p>该接口允许扩展管理自身属性、状态和生命周期。</p>
 */
public interface Extension
{
    /**
     * 设置当前扩展的显示名称。<br/>
     * 该名称将显示在Extensions工具的用户界面中，并用于标识持久化数据。
     *
     * @param extensionName 要设置的扩展名称
     * 
     * <p><b>注意：</b></p>
     * <ul>
     *   <li>名称应具有唯一性以避免混淆</li>
     *   <li>名称更改会立即生效并反映在UI中</li>
     *   <li>该名称也会用于扩展配置的持久化存储标识</li>
     * </ul>
     */
    void setName(String extensionName);

    /**
     * 获取当前扩展加载来源文件的绝对路径。
     *
     * @return 加载当前扩展的文件的绝对路径
     * 
     * <p><b>典型用途：</b></p>
     * <ul>
     *   <li>定位扩展相关的资源文件</li>
     *   <li>调试时确定扩展加载位置</li>
     * </ul>
     */
    String filename();

    /**
     * 判断当前扩展是否作为BApp(官方应用商店扩展)加载。
     *
     * @return 如果是BApp则返回{@code true}，否则返回{@code false}
     * 
     * <p><b>说明：</b></p>
     * <ul>
     *   <li>BApp是经过Burp官方认证的扩展</li>
     *   <li>此标志可用于实现BApp特有的逻辑</li>
     * </ul>
     */
    boolean isBapp();

    /**
     * 从Burp Suite中卸载当前扩展。
     * 
     * <p><b>重要说明：</b></p>
     * <ul>
     *   <li>此操作会立即终止扩展</li>
     *   <li>扩展应通过{@link #registerUnloadingHandler}注册卸载处理器来清理资源</li>
     *   <li>通常由用户通过UI触发，而非扩展自身调用</li>
     * </ul>
     */
    void unload();

    /**
     * 注册一个处理器，用于接收扩展状态变化的通知。<br>
     * <b>注意：</b>任何启动了后台线程或打开了系统资源(如文件或数据库连接)的扩展，
     * 都应注册此监听器，并在扩展卸载时终止线程/关闭资源。
     *
     * @param handler 扩展实现的{@link ExtensionUnloadingHandler}接口对象
     * 
     * @return 处理器的{@link Registration}注册对象，可用于取消注册
     * 
     * <p><b>最佳实践：</b></p>
     * <ul>
     *   <li>应在扩展初始化时尽早注册此处理器</li>
     *   <li>处理器中应实现所有必要的资源清理逻辑</li>
     *   <li>可通过返回的Registration对象在适当时机取消注册</li>
     * </ul>
     */
    Registration registerUnloadingHandler(ExtensionUnloadingHandler handler);
}
```
### ExtensionUnloadingHandler
```java
/*
 * Copyright (c) 2022-2023. PortSwigger Ltd. 保留所有权利。
 *
 * 此代码可用于扩展Burp Suite Community Edition和Burp Suite Professional的功能，
 * 前提是该使用不违反这些产品的许可条款。
 */

package burp.api.montoya.extension;

/**
 * 扩展卸载处理器接口。
 * <p>扩展可以实现此接口，并通过调用{@link Extension#registerUnloadingHandler(ExtensionUnloadingHandler)}注册扩展卸载处理器。
 * 当扩展被卸载时，处理器会收到通知。</p>
 * 
 * <p><b>重要说明：</b></p>
 * <ul>
 *   <li>任何启动了后台线程或打开了系统资源（如文件、数据库连接等）的扩展</li>
 *   <li>都应该注册此处理器</li>
 *   <li>并在扩展卸载时正确终止线程和释放资源</li>
 * </ul>
 * 
 * <p><b>典型使用场景：</b></p>
 * <ul>
 *   <li>关闭打开的数据库连接</li>
 *   <li>停止后台监控线程</li>
 *   <li>释放占用的系统资源</li>
 *   <li>保存临时状态数据</li>
 * </ul>
 */
public interface ExtensionUnloadingHandler
{
    /**
     * 当扩展被卸载时调用的方法。
     * 
     * <p><b>实现要求：</b></p>
     * <ul>
     *   <li>应在此方法中实现所有必要的清理逻辑</li>
     *   <li>方法执行时间应尽量短，避免阻塞卸载过程</li>
     *   <li>不应在此方法中抛出未捕获的异常</li>
     * </ul>
     * 
     * <p><b>示例实现：</b></p>
     * <pre>
     * public void extensionUnloaded() {
     *     // 停止后台线程
     *     backgroundThread.interrupt();
     *     
     *     // 关闭数据库连接
     *     if (dbConnection != null) {
     *         dbConnection.close();
     *     }
     *     
     *     // 释放其他资源
     *     resourceManager.cleanup();
     * }
     * </pre>
     */
    void extensionUnloaded();
}
```
## http
### Http
```java
/*
 * Copyright (c) 2022-2023. PortSwigger Ltd. 保留所有权利。
 *
 * 此代码可用于扩展Burp Suite Community Edition和Burp Suite Professional的功能，
 * 前提是该使用不违反这些产品的许可条款。
 */

package burp.api.montoya.http;

import burp.api.montoya.core.Registration;
import burp.api.montoya.http.handler.HttpHandler;
import burp.api.montoya.http.message.HttpRequestResponse;
import burp.api.montoya.http.message.requests.HttpRequest;
import burp.api.montoya.http.message.responses.analysis.ResponseKeywordsAnalyzer;
import burp.api.montoya.http.message.responses.analysis.ResponseVariationsAnalyzer;
import burp.api.montoya.http.sessions.CookieJar;
import burp.api.montoya.http.sessions.SessionHandlingAction;

import java.util.List;

/**
 * 提供对Burp Suite中HTTP相关功能的访问接口。
 * <p>该接口包含HTTP请求发送、响应处理、会话管理和分析等功能。</p>
 */
public interface Http
{
    /**
     * 注册HTTP请求/响应处理器。
     * <p>当任何Burp工具即将发送请求或收到响应时，会调用该处理器。</p>
     *
     * @param handler 实现了{@link HttpHandler}接口的扩展对象
     * @return 处理器的{@link Registration}注册对象，可用于取消注册
     *
     * <p><b>典型用途：</b></p>
     * <ul>
     *   <li>监控所有HTTP流量</li>
     *   <li>修改请求/响应内容</li>
     *   <li>记录特定请求信息</li>
     * </ul>
     */
    Registration registerHttpHandler(HttpHandler handler);

    /**
     * 注册自定义会话处理动作。
     * <p>每个注册的处理动作会在会话处理规则UI中显示，供用户选择作为规则动作。</p>
     *
     * @param sessionHandlingAction 实现了{@link SessionHandlingAction}接口的扩展对象
     * @return 处理动作的{@link Registration}注册对象
     *
     * <p><b>注意：</b></p>
     * <ul>
     *   <li>处理动作可以单独执行，也可以在宏执行后执行</li>
     *   <li>常用于处理自定义会话令牌或认证机制</li>
     * </ul>
     */
    Registration registerSessionHandlingAction(SessionHandlingAction sessionHandlingAction);

    /**
     * 发送HTTP请求并获取响应（使用默认模式和连接）。
     *
     * @param request 完整的HTTP请求
     * @return 包含请求和响应的{@link HttpRequestResponse}对象
     */
    HttpRequestResponse sendRequest(HttpRequest request);

    /**
     * 按指定模式发送HTTP请求并获取响应。
     *
     * @param request 完整的HTTP请求
     * @param httpMode 指定请求发送模式的{@link HttpMode}枚举值
     * @return 包含请求和响应的{@link HttpRequestResponse}对象
     */
    HttpRequestResponse sendRequest(HttpRequest request, HttpMode httpMode);

    /**
     * 按指定模式和连接发送HTTP请求并获取响应。
     *
     * @param request 完整的HTTP请求
     * @param httpMode 指定请求发送模式的{@link HttpMode}枚举值
     * @param connectionId 要使用的连接标识符
     * @return 包含请求和响应的{@link HttpRequestResponse}对象
     *
     * <p><b>连接复用说明：</b></p>
     * <ul>
     *   <li>相同connectionId的请求会复用TCP连接</li>
     *   <li>适用于需要保持会话的场景</li>
     * </ul>
     */
    HttpRequestResponse sendRequest(HttpRequest request, HttpMode httpMode, String connectionId);

    /**
     * 使用指定选项发送HTTP请求并获取响应。
     *
     * @param request 完整的HTTP请求
     * @param requestOptions 指定请求选项的{@link RequestOptions}对象
     * @return 包含请求和响应的{@link HttpRequestResponse}对象
     */
    HttpRequestResponse sendRequest(HttpRequest request, RequestOptions requestOptions);

    /**
     * 并行发送多个HTTP请求并获取响应列表（使用默认模式）。
     *
     * @param requests HTTP请求列表
     * @return 包含请求和响应的{@link HttpRequestResponse}对象列表
     */
    List<HttpRequestResponse> sendRequests(List<HttpRequest> requests);

    /**
     * 按指定模式并行发送多个HTTP请求并获取响应列表。
     *
     * @param requests HTTP请求列表
     * @param httpMode 指定请求发送模式的{@link HttpMode}枚举值
     * @return 包含请求和响应的{@link HttpRequestResponse}对象列表
     */
    List<HttpRequestResponse> sendRequests(List<HttpRequest> requests, HttpMode httpMode);

    /**
     * 创建响应关键词分析器。
     *
     * @param keywords 要分析的关键词列表
     * @return 新建的{@link ResponseKeywordsAnalyzer}实例
     *
     * <p><b>分析功能：</b></p>
     * <ul>
     *   <li>检测响应中是否包含指定关键词</li>
     *   <li>统计关键词出现频率</li>
     *   <li>适用于内容扫描和特征检测</li>
     * </ul>
     */
    ResponseKeywordsAnalyzer createResponseKeywordsAnalyzer(List<String> keywords);

    /**
     * 创建响应变化分析器。
     *
     * @return 新建的{@link ResponseVariationsAnalyzer}实例
     *
     * <p><b>分析功能：</b></p>
     * <ul>
     *   <li>分析多个响应之间的差异</li>
     *   <li>识别动态内容和固定内容</li>
     *   <li>适用于会话处理和输入验证测试</li>
     * </ul>
     */
    ResponseVariationsAnalyzer createResponseVariationsAnalyzer();

    /**
     * 获取Cookie Jar实例。
     *
     * @return {@link CookieJar}实例
     *
     * <p><b>功能说明：</b></p>
     * <ul>
     *   <li>访问和修改Burp的Cookie存储</li>
     *   <li>监控Cookie变化</li>
     *   <li>支持会话管理和认证测试</li>
     * </ul>
     */
    CookieJar cookieJar();
}

```
### HttpMode
```java
/*
 * Copyright (c) 2022-2023. PortSwigger Ltd. 保留所有权利。
 *
 * 此代码可用于扩展Burp Suite Community Edition和Burp Suite Professional的功能，
 * 前提是该使用不违反这些产品的许可条款。
 */

package burp.api.montoya.http;

/**
 * HTTP请求发送模式枚举。
 * <p>定义发送HTTP请求时使用的协议版本和协商方式。</p>
 */
public enum HttpMode
{
    /**
     * 自动模式（默认）：
     * <ul>
     *   <li>根据服务器支持的协议自动选择HTTP版本</li>
     *   <li>通过ALPN(应用层协议协商)确定最佳协议</li>
     *   <li>推荐在大多数情况下使用</li>
     * </ul>
     */
    AUTO,

    /**
     * 强制使用HTTP/1协议：
     * <ul>
     *   <li>仅使用HTTP/1.x版本通信</li>
     *   <li>如果服务器仅支持HTTP/2，将返回错误</li>
     *   <li>适用于需要兼容旧系统的场景</li>
     * </ul>
     */
    HTTP_1,

    /**
     * 强制使用HTTP/2协议：
     * <ul>
     *   <li>仅使用HTTP/2版本通信</li>
     *   <li>如果服务器仅支持HTTP/1.x，将返回错误</li>
     *   <li>适用于需要测试HTTP/2特性的场景</li>
     * </ul>
     */
    HTTP_2,

    /**
     * 强制使用HTTP/2并忽略ALPN：
     * <ul>
     *   <li>无条件使用HTTP/2协议</li>
     *   <li>即使服务器不支持HTTP/2也不会报错</li>
     *   <li>服务器可能会降级到HTTP/1.x</li>
     *   <li>适用于特殊测试场景</li>
     *   <li><b>注意：</b>可能导致非标准协议交互</li>
     * </ul>
     */
    HTTP_2_IGNORE_ALPN
}
```
### HttpProtocol
```java
/*
 * Copyright (c) 2022-2023. PortSwigger Ltd. All rights reserved.
 * 版权所有 (c) 2022-2023。PortSwigger Ltd. 保留所有权利。
 *
 * This code may be used to extend the functionality of Burp Suite Community Edition
 * and Burp Suite Professional, provided that this usage does not violate the
 * license terms for those products.
 * 此代码可用于扩展 Burp Suite 社区版和 Burp Suite 专业版的功能，
 * 前提是该使用不违反这些产品的许可条款。
 */

package burp.api.montoya.http;

/**
 * HTTP protocols.
 * HTTP协议枚举
 */
public enum HttpProtocol
{
    /**
     * Hypertext Transfer Protocol
     * 超文本传输协议（HTTP）
     */
    HTTP,
    
    /**
     * Hypertext Transfer Protocol Secure
     * 安全超文本传输协议（HTTPS）
     */
    HTTPS
}

```
###  HttpService
```java
/*
 * Copyright (c) 2022-2023. PortSwigger Ltd. All rights reserved.
 *
 * 此代码可用于扩展 Burp Suite Community Edition 和 Burp Suite Professional 的功能，
 * 前提是该使用不违反这些产品的许可条款。
 */

package burp.api.montoya.http;

import static burp.api.montoya.internal.ObjectFactoryLocator.FACTORY;

/**
 * Burp HTTP 服务，提供有关可以发送 HTTP 请求的 HTTP 服务的详细信息。
 */
public interface HttpService
{
    /**
     * 获取服务的主机名或IP地址
     * 
     * @return 服务的主机名或IP地址
     */
    String host();

    /**
     * 获取服务的端口号
     * 
     * @return 服务的端口号
     */
    int port();

    /**
     * 检查是否使用安全协议进行连接
     * 
     * @return 如果使用安全协议返回 true，否则返回 false
     */
    boolean secure();

    /**
     * 动态解析主机名为IP地址
     * 
     * @return 主机的IP地址
     */
    String ipAddress();

    /**
     * 获取服务的字符串表示形式
     * 
     * @return 服务的字符串表示
     */
    @Override
    String toString();

    /**
     * 创建新的 {@code HttpService} 实例
     *
     * @param baseUrl 服务的URL
     * @return 新的 {@code HttpService} 实例
     * @throws IllegalArgumentException 如果提供的URL无效
     */
    static HttpService httpService(String baseUrl)
    {
        return FACTORY.httpService(baseUrl);
    }

    /**
     * 创建新的 {@code HttpService} 实例
     *
     * @param host 服务的主机名或IP地址
     * @param secure 是否使用安全连接
     * @return 新的 {@code HttpService} 实例
     */
    static HttpService httpService(String host, boolean secure)
    {
        return FACTORY.httpService(host, secure);
    }

    /**
     * 创建新的 {@code HttpService} 实例
     *
     * @param host 服务的主机名或IP地址
     * @param port 服务的端口号
     * @param secure 是否使用安全连接
     * @return 新的 {@code HttpService} 实例
     */
    static HttpService httpService(String host, int port, boolean secure)
    {
        return FACTORY.httpService(host, port, secure);
    }
}
```
### RedirectionMode
```java
package burp.api.montoya.http;

/**
 * 发送请求时的重定向模式
 */
public enum RedirectionMode
{
    /**
     * 始终跟随重定向
     */
    ALWAYS,
    
    /**
     * 从不跟随重定向
     */
    NEVER,
    
    /**
     * 仅当重定向到相同主机时才跟随
     */
    SAME_HOST,
    
    /**
     * 仅当重定向目标在范围内时才跟随
     */
    IN_SCOPE
}
```
### RequestOptions
```java
package burp.api.montoya.http;

import static burp.api.montoya.internal.ObjectFactoryLocator.FACTORY;

/**
 * 用于指定HTTP请求选项的接口
 */
public interface RequestOptions
{
    /**
     * 设置发送请求时使用的HTTP模式
     *
     * @param httpMode {@link HttpMode} 枚举值，指示请求应该如何发送
     * @return 当前请求选项实例（支持链式调用）
     */
    RequestOptions withHttpMode(HttpMode httpMode);

    /**
     * 设置通过特定连接发送请求时的连接ID
     *
     * @param connectionId 要使用的连接标识符
     * @return 当前请求选项实例（支持链式调用）
     */
    RequestOptions withConnectionId(String connectionId);

    /**
     * 启用发送请求时的上游TLS验证
     *
     * @return 当前请求选项实例（支持链式调用）
     */
    RequestOptions withUpstreamTLSVerification();

    /**
     * 设置发送请求时使用的重定向模式
     *
     * @param redirectionMode {@link RedirectionMode} 枚举值，指示如何处理重定向
     * @return 当前请求选项实例（支持链式调用）
     */
    RequestOptions withRedirectionMode(RedirectionMode redirectionMode);

    /**
     * 设置发送请求时使用的服务器名称指示(SNI)
     *
     * @param serverNameIndicator 要使用的服务器名称指示
     * @return 当前请求选项实例（支持链式调用）
     */
    RequestOptions withServerNameIndicator(String serverNameIndicator);

    /**
     * 设置读取响应时使用的超时时间
     *
     * @param timeoutMs 超时时间（毫秒），0表示无超时
     * @return 当前请求选项实例（支持链式调用）
     */
    RequestOptions withResponseTimeout(long timeoutMs);

    /**
     * 获取新的RequestOptions实例
     *
     * @return 新的RequestOptions实例
     */
    static RequestOptions requestOptions()
    {
        return FACTORY.requestOptions();
    }
}
```
### RequestResponseSelection
```java
package burp.api.montoya.http;

import burp.api.montoya.ui.Selection;

/**
 * 提供对HTTP请求/响应中用户选中内容及其起始/结束位置的访问
 */
public interface RequestResponseSelection 
{
    /**
     * 获取HTTP请求中的用户选中内容及其位置信息
     * @return 请求中的选中内容对象（包含内容和位置信息）
     */
    Selection requestSelection();

    /**
     * 获取HTTP响应中的用户选中内容及其位置信息
     * @return 响应中的选中内容对象（包含内容和位置信息）
     */
    Selection responseSelection();

    /**
     * 检查HTTP请求中是否存在用户选中的内容
     * @return 如果请求中有选中内容则返回true，否则false
     */
    boolean hasRequestSelection();

    /**
     * 检查HTTP响应中是否存在用户选中的内容
     * @return 如果响应中有选中内容则返回true，否则false
     */
    boolean hasResponseSelection();
}
```
### handler
#### HttpHandler
```java
/*
 * 版权所有 (c) 2022-2023。PortSwigger Ltd. 保留所有权利。
 *
 * 此代码可用于扩展 Burp Suite Community Edition 和 Burp Suite Professional 的功能，
 * 前提是该使用不违反这些产品的许可条款。
 */

package burp.api.montoya.http.handler;

import burp.api.montoya.http.Http;

/**
 * 扩展可以实现此接口，并通过调用 {@link Http#registerHttpHandler} 注册HTTP处理器。
 * 该处理器将收到所有Burp工具发出和接收的HTTP请求/响应通知。
 * 扩展可以通过注册HTTP处理器对这些消息执行自定义分析或修改。
 */
public interface HttpHandler
{
    /**
     * 当HTTP请求即将发送时由Burp调用
     *
     * @param requestToBeSent 包含即将发送的HTTP请求信息
     * @return {@link RequestToBeSentAction} 实例，决定对请求的处理动作
     */
    RequestToBeSentAction handleHttpRequestToBeSent(HttpRequestToBeSent requestToBeSent);

    /**
     * 当HTTP响应被接收时由Burp调用
     *
     * @param responseReceived 包含接收到的HTTP响应信息
     * @return {@link ResponseReceivedAction} 实例，决定对响应的处理动作
     */
    ResponseReceivedAction handleHttpResponseReceived(HttpResponseReceived responseReceived);
}
```
#### HttpRequestToBeSent
```java
/*
 * 版权所有 (c) 2022-2023。PortSwigger Ltd. 保留所有权利。
 *
 * 此代码可用于扩展 Burp Suite Community Edition 和 Burp Suite Professional 的功能，
 * 前提是该使用不违反这些产品的许可条款。
 */

package burp.api.montoya.http.handler;

import burp.api.montoya.core.Annotations;
import burp.api.montoya.core.ByteArray;
import burp.api.montoya.core.Marker;
import burp.api.montoya.core.ToolSource;
import burp.api.montoya.http.HttpService;
import burp.api.montoya.http.message.ContentType;
import burp.api.montoya.http.message.HttpHeader;
import burp.api.montoya.http.message.params.HttpParameter;
import burp.api.montoya.http.message.params.HttpParameterType;
import burp.api.montoya.http.message.params.ParsedHttpParameter;
import burp.api.montoya.http.message.requests.HttpRequest;
import burp.api.montoya.http.message.requests.HttpTransformation;
import burp.api.montoya.http.message.requests.MalformedRequestException;

import java.util.List;
import java.util.regex.Pattern;

/**
 * 表示即将发送的HTTP请求，扩展了{@link HttpRequest}接口，
 * 提供获取请求注解和工具来源的额外方法。
 */
public interface HttpRequestToBeSent extends HttpRequest
{
    /**
     * 获取此请求的唯一消息ID（对应的响应将具有相同的ID）
     * @return 消息ID
     */
    int messageId();

    /**
     * 获取请求/响应的注解信息
     * @return 注解对象
     */
    Annotations annotations();

    /**
     * 获取发送此请求的Burp工具来源
     * @return 工具来源对象
     */
    ToolSource toolSource();

    /**
     * 检查请求是否在目标范围内
     * @return 如果在范围内返回true，否则false
     */
    @Override
    boolean isInScope();

    /**
     * 获取请求的HTTP服务信息
     * @return HTTP服务对象
     */
    @Override
    HttpService httpService();

    /**
     * 获取请求的完整URL
     * @return URL字符串
     * @throws MalformedRequestException 如果请求格式错误
     */
    @Override
    String url() throws MalformedRequestException;

    /**
     * 获取请求的HTTP方法
     * @return HTTP方法（如GET/POST等）
     * @throws MalformedRequestException 如果请求格式错误
     */
    @Override
    String method() throws MalformedRequestException;

    /**
     * 获取请求路径（包含查询参数）
     * @return 路径字符串
     * @throws MalformedRequestException 如果请求格式错误
     */
    @Override
    String path() throws MalformedRequestException;

    /**
     * 获取请求路径（不包含查询参数）
     * @return 路径字符串
     * @throws MalformedRequestException 如果请求格式错误
     */
    @Override
    String pathWithoutQuery() throws MalformedRequestException;

    /**
     * 获取HTTP协议版本
     * @return 版本字符串（如"HTTP/1.1"或"HTTP/2"）
     */
    @Override
    String httpVersion();

    /**
     * 获取所有HTTP头信息
     * @return HTTP头列表
     */
    @Override
    List<HttpHeader> headers();

    /**
     * 检查是否包含指定头
     * @param header 要检查的头对象
     * @return 如果包含返回true
     */
    @Override
    boolean hasHeader(HttpHeader header);

    /**
     * 检查是否包含指定名称的头
     * @param name 头名称
     * @return 如果包含返回true
     */
    @Override
    boolean hasHeader(String name);

    /**
     * 检查是否包含指定名称和值的头
     * @param name 头名称
     * @param value 头值
     * @return 如果包含返回true
     */
    @Override
    boolean hasHeader(String name, String value);

    /**
     * 获取指定名称的头
     * @param name 头名称
     * @return 头对象，如果不存在返回null
     */
    @Override
    HttpHeader header(String name);

    /**
     * 获取指定名称头的值
     * @param name 头名称
     * @return 头值字符串，如果不存在返回null
     */
    @Override
    String headerValue(String name);

    /**
     * 检查请求是否包含参数
     * @return 如果包含参数返回true
     */
    @Override
    boolean hasParameters();

    /**
     * 检查请求是否包含指定类型的参数
     * @param type 参数类型
     * @return 如果包含返回true
     */
    @Override
    boolean hasParameters(HttpParameterType type);

    /**
     * 获取指定名称和类型的参数
     * @param name 参数名
     * @param type 参数类型
     * @return 参数对象，如果不存在返回null
     */
    @Override
    ParsedHttpParameter parameter(String name, HttpParameterType type);

    /**
     * 获取指定名称和类型参数的值
     * @param name 参数名
     * @param type 参数类型
     * @return 参数值，如果不存在返回null
     */
    @Override
    String parameterValue(String name, HttpParameterType type);

    /**
     * 检查是否存在指定名称和类型的参数
     * @param name 参数名
     * @param type 参数类型
     * @return 如果存在返回true
     */
    @Override
    boolean hasParameter(String name, HttpParameterType type);

    /**
     * 检查是否存在与给定参数匹配的参数
     * @param parameter 要匹配的参数
     * @return 如果存在返回true
     */
    @Override
    boolean hasParameter(HttpParameter parameter);

    /**
     * 获取请求的内容类型
     * @return 内容类型枚举
     */
    @Override
    ContentType contentType();

    /**
     * 获取所有参数列表
     * @return 参数列表
     */
    @Override
    List<ParsedHttpParameter> parameters();

    /**
     * 获取指定类型的参数列表
     * @param type 参数类型
     * @return 过滤后的参数列表
     */
    @Override
    List<ParsedHttpParameter> parameters(HttpParameterType type);

    /**
     * 获取消息体字节数组
     * @return 消息体字节数组
     */
    @Override
    ByteArray body();

    /**
     * 获取消息体字符串
     * @return 消息体字符串
     */
    @Override
    String bodyToString();

    /**
     * 获取消息体起始偏移量
     * @return 偏移量值
     */
    @Override
    int bodyOffset();

    /**
     * 获取所有标记列表
     * @return 标记列表
     */
    @Override
    List<Marker> markers();

    /**
     * 检查是否包含搜索词
     * @param searchTerm 搜索词
     * @param caseSensitive 是否区分大小写
     * @return 如果包含返回true
     */
    @Override
    boolean contains(String searchTerm, boolean caseSensitive);

    /**
     * 检查是否匹配正则表达式
     * @param pattern 正则表达式
     * @return 如果匹配返回true
     */
    @Override
    boolean contains(Pattern pattern);

    /**
     * 获取整个消息的字节数组
     * @return 消息字节数组
     */
    @Override
    ByteArray toByteArray();

    /**
     * 获取整个消息的字符串表示
     * @return 消息字符串
     */
    @Override
    String toString();

    /**
     * 创建请求的临时文件副本（用于长期存储）
     * @return 新的HttpRequest实例
     */
    HttpRequest copyToTempFile();

    /**
     * 创建使用新服务的请求副本
     * @param service HTTP服务对象
     * @return 新的HttpRequest实例
     */
    @Override
    HttpRequest withService(HttpService service);

    /**
     * 创建使用新路径的请求副本
     * @param path 新路径
     * @return 新的HttpRequest实例
     */
    @Override
    HttpRequest withPath(String path);

    /**
     * 创建使用新方法的请求副本
     * @param method 新方法
     * @return 新的HttpRequest实例
     */
    @Override
    HttpRequest withMethod(String method);

    /**
     * 创建添加/更新头后的请求副本
     * @param header HTTP头对象
     * @return 新的HttpRequest实例
     */
    @Override
    HttpRequest withHeader(HttpHeader header);

    /**
     * 创建添加/更新头后的请求副本
     * @param name 头名称
     * @param value 头值
     * @return 新的HttpRequest实例
     */
    @Override
    HttpRequest withHeader(String name, String value);

    /**
     * 创建添加/更新参数后的请求副本
     * @param parameters HTTP参数
     * @return 新的HttpRequest实例
     */
    @Override
    HttpRequest withParameter(HttpParameter parameters);

    /**
     * 创建添加多个参数后的请求副本
     * @param parameters HTTP参数列表
     * @return 新的HttpRequest实例
     */
    @Override
    HttpRequest withAddedParameters(List<? extends HttpParameter> parameters);

    /**
     * 创建添加多个参数后的请求副本
     * @param parameters HTTP参数数组
     * @return 新的HttpRequest实例
     */
    @Override
    HttpRequest withAddedParameters(HttpParameter... parameters);

    /**
     * 创建移除多个参数后的请求副本
     * @param parameters 要移除的参数列表
     * @return 新的HttpRequest实例
     */
    @Override
    HttpRequest withRemovedParameters(List<? extends HttpParameter> parameters);

    /**
     * 创建移除多个参数后的请求副本
     * @param parameters 要移除的参数数组
     * @return 新的HttpRequest实例
     */
    @Override
    HttpRequest withRemovedParameters(HttpParameter... parameters);

    /**
     * 创建更新多个参数后的请求副本
     * @param parameters 要更新的参数列表
     * @return 新的HttpRequest实例
     */
    @Override
    HttpRequest withUpdatedParameters(List<? extends HttpParameter> parameters);

    /**
     * 创建更新多个参数后的请求副本
     * @param parameters 要更新的参数数组
     * @return 新的HttpRequest实例
     */
    @Override
    HttpRequest withUpdatedParameters(HttpParameter... parameters);

    /**
     * 创建应用转换后的请求副本
     * @param transformation 转换对象
     * @return 新的HttpRequest实例
     */
    @Override
    HttpRequest withTransformationApplied(HttpTransformation transformation);

    /**
     * 创建更新消息体后的请求副本
     * @param body 新消息体字符串
     * @return 新的HttpRequest实例
     */
    @Override
    HttpRequest withBody(String body);

    /**
     * 创建更新消息体后的请求副本
     * @param body 新消息体字节数组
     * @return 新的HttpRequest实例
     */
    @Override
    HttpRequest withBody(ByteArray body);

    /**
     * 创建添加头后的请求副本
     * @param name 头名称
     * @param value 头值
     * @return 新的HttpRequest实例
     */
    @Override
    HttpRequest withAddedHeader(String name, String value);

    /**
     * 创建添加头后的请求副本
     * @param header HTTP头对象
     * @return 新的HttpRequest实例
     */
    @Override
    HttpRequest withAddedHeader(HttpHeader header);

    /**
     * 创建更新头后的请求副本
     * @param name 要更新的头名称
     * @param value 新头值
     * @return 新的HttpRequest实例
     */
    @Override
    HttpRequest withUpdatedHeader(String name, String value);

    /**
     * 创建更新头后的请求副本
     * @param header 包含新值的HTTP头对象
     * @return 新的HttpRequest实例
     */
    @Override
    HttpRequest withUpdatedHeader(HttpHeader header);

    /**
     * 创建移除头后的请求副本
     * @param name 要移除的头名称
     * @return 新的HttpRequest实例
     */
    @Override
    HttpRequest withRemovedHeader(String name);

    /**
     * 创建移除头后的请求副本
     * @param header 要移除的HTTP头对象
     * @return 新的HttpRequest实例
     */
    @Override
    HttpRequest withRemovedHeader(HttpHeader header);

    /**
     * 创建添加标记后的请求副本
     * @param markers 标记列表
     * @return 新的HttpRequest实例
     */
    @Override
    HttpRequest withMarkers(List<Marker> markers);

    /**
     * 创建添加标记后的请求副本
     * @param markers 标记数组
     * @return 新的HttpRequest实例
     */
    @Override
    HttpRequest withMarkers(Marker... markers);

    /**
     * 创建添加默认头后的请求副本
     * @return 新的HttpRequest实例
     */
    @Override
    HttpRequest withDefaultHeaders();
}
```
#### HttpResponseReceived
```java
/**
 * Burp Suite 的 HTTP 响应接收接口，继承自 {@link HttpResponse}，并增加了获取发起请求、注解和工具来源的方法。
 * 该接口用于表示 Burp Suite 接收到的 HTTP 响应，包含响应内容以及与请求相关的元数据。
 */
public interface HttpResponseReceived extends HttpResponse {

    /**
     * 获取该响应对应的消息 ID，该 ID 与对应的请求 ID 相同。
     *
     * @return 消息 ID
     */
    int messageId();

    /**
     * 获取发起该响应的 HTTP 请求。
     *
     * @return 发起请求的 {@link HttpRequest} 对象
     */
    HttpRequest initiatingRequest();

    /**
     * 获取该请求/响应的注解信息。
     *
     * @return 包含注解信息的 {@link Annotations} 对象
     */
    Annotations annotations();

    /**
     * 获取发送该请求的 Burp Suite 工具来源。
     *
     * @return 工具来源的 {@link ToolSource} 对象
     */
    ToolSource toolSource();

    // ========== 继承自 HttpResponse 的方法 ==========

    /**
     * 获取响应中的 HTTP 状态码。
     *
     * @return HTTP 状态码
     */
    @Override
    short statusCode();

    /**
     * 获取响应中的 HTTP 原因短语（HTTP/1.x 消息）。
     * 对于 HTTP/2 消息，将根据状态码返回映射的原因短语。
     *
     * @return HTTP 原因短语
     */
    @Override
    String reasonPhrase();

    /**
     * 检查状态码是否属于指定的状态码类别。
     *
     * @param statusCodeClass 要检查的状态码类别
     * @return 如果状态码属于指定类别，则返回 true
     */
    @Override
    boolean isStatusCodeClass(StatusCodeClass statusCodeClass);

    /**
     * 获取 HTTP 版本文本（从响应行解析，HTTP/1.x 消息）。
     * 对于 HTTP/2 消息，返回 "HTTP/2"。
     *
     * @return HTTP 版本字符串
     */
    @Override
    String httpVersion();

    /**
     * 获取消息中包含的所有 HTTP 头。
     *
     * @return HTTP 头列表
     */
    @Override
    List<HttpHeader> headers();

    /**
     * 检查消息中是否包含指定的 HTTP 头。
     *
     * @param header 要检查的 HTTP 头
     * @return 如果存在则返回 true
     */
    @Override
    boolean hasHeader(HttpHeader header);

    /**
     * 检查消息中是否包含指定名称的 HTTP 头。
     *
     * @param name 要检查的 HTTP 头名称
     * @return 如果存在则返回 true
     */
    @Override
    boolean hasHeader(String name);

    /**
     * 检查消息中是否包含指定名称和值的 HTTP 头。
     *
     * @param name  要检查的 HTTP 头名称
     * @param value 要检查的 HTTP 头值
     * @return 如果存在匹配的头则返回 true
     */
    @Override
    boolean hasHeader(String name, String value);

    // ...（其他方法注释类似，此处省略以保持简洁）

    /**
     * 创建带有新状态码的 HTTP 响应副本。
     *
     * @param statusCode 新的状态码
     * @return 新的 {@code HttpResponse} 实例
     */
    @Override
    HttpResponse withStatusCode(short statusCode);

    /**
     * 创建带有新原因短语的 HTTP 响应副本。
     *
     * @param reasonPhrase 新的原因短语
     * @return 新的 {@code HttpResponse} 实例
     */
    @Override
    HttpResponse withReasonPhrase(String reasonPhrase);

    /**
     * 创建带有新 HTTP 版本的 HTTP 响应副本。
     *
     * @param httpVersion 新的 HTTP 版本
     * @return 新的 {@code HttpResponse} 实例
     */
    @Override
    HttpResponse withHttpVersion(String httpVersion);

    // ...（其他 withXXX 方法注释类似）

    /**
     * 创建带有新增标记的 HTTP 响应副本。
     *
     * @param markers 要添加的标记（可变参数）
     * @return 新的 {@code MarkedHttpRequestResponse} 实例
     */
    @Override
    HttpResponse withMarkers(Marker... markers);
}
```
#### RequestAction
```java
/*
 * 版权所有 (c) 2022-2023。PortSwigger Ltd. 保留所有权利。
 *
 * 此代码可用于扩展 Burp Suite Community Edition 和 Burp Suite Professional 的功能，
 * 前提是该使用不违反这些产品的许可条款。
 */

package burp.api.montoya.http.handler;

/**
 * HTTP请求拦截时采取的操作枚举
 * <p>
 * 用于决定在拦截HTTP请求后的处理行为
 */
public enum RequestAction
{
    /**
     * 继续处理并发送该请求
     * <p>
     * 使用此选项将允许Burp正常发送被拦截的请求
     */
    CONTINUE
}
```
#### RequestToBeSentAction
```java
/*
 * Copyright (c) 2022-2023. PortSwigger Ltd. All rights reserved.
 *
 * 本代码可用于扩展 Burp Suite Community Edition 和 Burp Suite Professional 的功能，
 * 前提是该使用不违反这些产品的许可条款。
 */

package burp.api.montoya.http.handler;

import burp.api.montoya.core.Annotations;
import burp.api.montoya.http.message.requests.HttpRequest;

import static burp.api.montoya.internal.ObjectFactoryLocator.FACTORY;

/**
 * 当自定义的 {@link HttpHandler} 注册到 Burp 后，{@link HttpHandler#handleHttpRequestToBeSent} 
 * 方法应返回此接口的实例。
 * 
 * <p>该接口用于处理即将发送的 HTTP 请求，允许修改请求内容或注解信息。</p>
 */
public interface RequestToBeSentAction
{
    /**
     * 获取要执行的操作类型。
     * 
     * <p>默认返回 {@code RequestAction.CONTINUE} 表示继续处理请求。</p>
     *
     * @return 请求操作类型
     */
    default RequestAction action()
    {
        return RequestAction.CONTINUE;
    }

    /**
     * 获取 HTTP 请求对象。
     *
     * @return HTTP 请求对象
     */
    HttpRequest request();

    /**
     * 获取与请求关联的注解信息。
     *
     * @return 注解对象
     */
    Annotations annotations();

    /**
     * 创建一个新的 {@code RequestResult} 实例（不修改注解）。
     *
     * @param request 要发送的 HTTP 请求
     * @return 新的请求处理结果实例
     */
    static RequestToBeSentAction continueWith(HttpRequest request)
    {
        return FACTORY.requestResult(request);
    }

    /**
     * 创建一个新的 {@code RequestResult} 实例（可修改注解）。
     *
     * @param request     要发送的 HTTP 请求
     * @param annotations 修改后的注解信息
     * @return 新的请求处理结果实例
     */
    static RequestToBeSentAction continueWith(HttpRequest request, Annotations annotations)
    {
        return FACTORY.requestResult(request, annotations);
    }
}
```
#### ResponseAction
```java
/*
 * 版权所有 (c) 2022-2023。PortSwigger Ltd. 保留所有权利。
 *
 * 此代码可用于扩展 Burp Suite Community Edition 和 Burp Suite Professional 的功能，
 * 前提是该使用不违反这些产品的许可条款。
 */

package burp.api.montoya.http.handler;

/**
 * HTTP响应拦截时采取的处理动作
 * <p>
 * 该枚举定义了在拦截到HTTP响应后可执行的操作类型
 */
public enum ResponseAction 
{
    /**
     * 继续处理并发送该响应
     * <p>
     * 使用此选项将指示Burp继续处理流程，正常发送被拦截的HTTP响应
     */
    CONTINUE
}
```
#### ResponseReceivedAction
```java
/*
 * 版权所有 (c) 2022-2023。PortSwigger Ltd. 保留所有权利。
 *
 * 此代码可用于扩展 Burp Suite Community Edition 和 Burp Suite Professional 的功能，
 * 前提是该使用不违反这些产品的许可条款。
 */

package burp.api.montoya.http.handler;

import burp.api.montoya.core.Annotations;
import burp.api.montoya.http.message.responses.HttpResponse;

import static burp.api.montoya.internal.ObjectFactoryLocator.FACTORY;

/**
 * HTTP响应接收后的处理动作
 * <p>
 * 当注册了自定义的{@link HttpHandler}时，应通过{@link HttpHandler#handleHttpResponseReceived}方法返回此接口的实例
 */
public interface ResponseReceivedAction
{
    /**
     * 获取要执行的响应动作
     * <p>默认返回{@link ResponseAction#CONTINUE}表示继续处理响应</p>
     * @return 响应动作枚举值
     */
    default ResponseAction action()
    {
        return ResponseAction.CONTINUE;
    }

    /**
     * 获取HTTP响应对象
     * @return HTTP响应实例
     */
    HttpResponse response();

    /**
     * 获取响应注解信息
     * @return 注解对象
     */
    Annotations annotations();

    /**
     * 创建新的响应结果实例（不修改注解）
     *
     * @param response HTTP响应对象
     * @return 新的响应结果实例
     */
    static ResponseReceivedAction continueWith(HttpResponse response)
    {
        return FACTORY.responseResult(response);
    }

    /**
     * 创建新的响应结果实例（包含修改后的注解）
     *
     * @param response    HTTP响应对象
     * @param annotations 修改后的注解对象
     * @return 新的响应结果实例
     */
    static ResponseReceivedAction continueWith(HttpResponse response, Annotations annotations)
    {
        return FACTORY.responseResult(response, annotations);
    }
}
```
####  TimingData
```java
/*
 * 版权所有 (c) 2022-2023。PortSwigger Ltd. 保留所有权利。
 *
 * 此代码可用于扩展 Burp Suite Community Edition 和 Burp Suite Professional 的功能，
 * 前提是该使用不违反这些产品的许可条款。
 */

package burp.api.montoya.http.handler;

import java.time.Duration;
import java.time.ZonedDateTime;

/**
 * HTTP请求/响应计时数据接口
 * <p>
 * 提供HTTP事务处理过程中的精确时间测量数据
 */
public interface TimingData
{
    /**
     * 获取从请求发送到响应开始接收的时间间隔
     * <p>
     * 测量从Burp发送请求到接收到响应第一个字节的时间差
     *
     * @return 时间间隔Duration对象，如果没有响应返回null
     */
    Duration timeBetweenRequestSentAndStartOfResponse();

    /**
     * 获取从请求发送到响应完全接收的时间间隔
     * <p>
     * 测量从Burp发送请求到完全接收响应的时间差
     *
     * @return 时间间隔Duration对象，如果没有响应或响应未完成返回null
     */
    Duration timeBetweenRequestSentAndEndOfResponse();

    /**
     * 获取请求发送的精确时间
     *
     * @return 请求发送的时间戳（带时区信息）
     */
    ZonedDateTime timeRequestSent();
}
```
### message
#### ContentType
```java
/*
 * 版权所有 (c) 2022-2023。PortSwigger Ltd. 保留所有权利。
 *
 * 此代码可用于扩展 Burp Suite Community Edition 和 Burp Suite Professional 的功能，
 * 前提是该使用不违反这些产品的许可条款。
 */

package burp.api.montoya.http.message;

/**
 * Burp 识别的 HTTP 内容类型枚举
 * <p>
 * 表示 HTTP 消息中常见的内容类型(MIME类型)分类
 */
public enum ContentType
{
    /**
     * 无内容类型（空内容或未指定）
     */
    NONE,
    
    /**
     * 未知内容类型（无法识别的类型）
     */
    UNKNOWN,
    
    /**
     * Action Message Format (AMF) - Adobe 的二进制协议格式
     */
    AMF,
    
    /**
     * JSON 格式数据 (application/json)
     */
    JSON,
    
    /**
     * 多部分表单数据 (multipart/form-data)
     */
    MULTIPART,
    
    /**
     * URL 编码表单数据 (application/x-www-form-urlencoded)
     */
    URL_ENCODED,
    
    /**
     * XML 格式数据 (application/xml 或 text/xml)
     */
    XML
}

```
#### Cookie
```java
/*
 * 版权所有 (c) 2022-2023。PortSwigger Ltd. 保留所有权利。
 *
 * 此代码可用于扩展 Burp Suite Community Edition 和 Burp Suite Professional 的功能，
 * 前提是该使用不违反这些产品的许可条款。
 */

package burp.api.montoya.http.message;

import burp.api.montoya.http.message.responses.HttpResponse;

import java.time.ZonedDateTime;
import java.util.Optional;

/**
 * Burp Cookie 接口
 * <p>
 * 用于获取和存储 Cookie 的详细信息
 */
public interface Cookie
{
    /**
     * 获取 Cookie 的名称
     * @return Cookie 名称字符串
     */
    String name();

    /**
     * 获取 Cookie 的值
     * @return Cookie 值字符串
     */
    String value();

    /**
     * 获取 Cookie 的作用域域名
     * <p>
     * <b>注意：</b>对于从生成的响应中获取的 Cookie（通过调用 {@link HttpResponse#httpResponse} 
     * 然后 {@link HttpResponse#cookies}），如果响应没有明确设置 Cookie 的 domain 属性，
     * 则返回的 domain 将为 {@code null}。
     *
     * @return Cookie 的作用域域名，可能为 null
     */
    String domain();

    /**
     * 获取 Cookie 的作用域路径
     * @return Cookie 的作用域路径，如果没有设置则返回 {@code null}
     */
    String path();

    /**
     * 获取 Cookie 的过期时间（如果可用）
     * <p>
     * 对于非持久化的会话 Cookie，可能没有过期时间
     *
     * @return 包含过期时间的 Optional 对象，可能为空
     */
    Optional<ZonedDateTime> expiration();
}
```
#### HttpHeader
```java
/*
 * 版权所有 (c) 2022-2023。PortSwigger Ltd. 保留所有权利。
 *
 * 此代码可用于扩展 Burp Suite Community Edition 和 Burp Suite Professional 的功能，
 * 前提是该使用不违反这些产品的许可条款。
 */

package burp.api.montoya.http.message;

import static burp.api.montoya.internal.ObjectFactoryLocator.FACTORY;

/**
 * Burp HTTP 头部接口
 * <p>
 * 用于表示和操作 HTTP 请求/响应头部的名称和值
 */
public interface HttpHeader
{
    /**
     * 获取 HTTP 头部的名称
     * @return 头部名称字符串
     */
    String name();

    /**
     * 获取 HTTP 头部的值
     * @return 头部值字符串
     */
    String value();

    /**
     * 获取 HTTP 头部的字符串表示形式（"名称: 值"格式）
     * @return 头部完整字符串
     */
    @Override
    String toString();

    /**
     * 通过名称和值创建新的 HTTP 头部实例
     *
     * @param name  头部名称
     * @param value 头部值
     * @return 新的 HttpHeader 实例
     */
    static HttpHeader httpHeader(String name, String value)
    {
        return FACTORY.httpHeader(name, value);
    }

    /**
     * 通过字符串创建新的 HTTP 头部实例
     * <p>
     * 字符串将按照 HTTP/1.1 规范解析为头部名称和值
     *
     * @param header 完整头部字符串（格式为"名称:值"）
     * @return 新的 HttpHeader 实例
     * @throws IllegalArgumentException 如果头部字符串格式不符合规范
     */
    static HttpHeader httpHeader(String header)
    {
        return FACTORY.httpHeader(header);
    }
}
```
#### HttpMessage
```java
/*
 * 版权所有 (c) 2022-2023。PortSwigger Ltd. 保留所有权利。
 *
 * 此代码可用于扩展 Burp Suite Community Edition 和 Burp Suite Professional 的功能，
 * 前提是该使用不违反这些产品的许可条款。
 */

package burp.api.montoya.http.message;

import burp.api.montoya.core.ByteArray;
import burp.api.montoya.core.Marker;
import burp.api.montoya.http.message.requests.HttpRequest;
import burp.api.montoya.http.message.responses.HttpResponse;

import java.util.List;
import java.util.regex.Pattern;

/**
 * HTTP 消息通用接口
 * <p>
 * 定义 {@link HttpRequest} 和 {@link HttpResponse} 共有的操作方法，
 * 提供对HTTP消息头部、正文和标记的统一访问方式
 */
public interface HttpMessage
{
    /**
     * 检查消息中是否包含指定头部
     * @param header 要检查的头部对象
     * @return 如果存在返回 true
     */
    boolean hasHeader(HttpHeader header);

    /**
     * 检查消息中是否包含指定名称的头部
     * @param name 头部名称
     * @return 如果存在返回 true
     */
    boolean hasHeader(String name);

    /**
     * 检查消息中是否包含指定名称和值的头部
     * @param name 头部名称
     * @param value 头部值
     * @return 如果匹配返回 true
     */
    boolean hasHeader(String name, String value);

    /**
     * 获取指定名称的头部对象
     * @param name 头部名称
     * @return 匹配的头部对象，未找到返回 null
     */
    HttpHeader header(String name);

    /**
     * 获取指定名称头部的值
     * @param name 头部名称
     * @return 头部值字符串，未找到返回 null
     */
    String headerValue(String name);

    /**
     * 获取消息中的所有头部列表
     * @return HTTP 头部列表
     */
    List<HttpHeader> headers();

    /**
     * 获取HTTP协议版本
     * <p>对于HTTP/1.x消息返回"HTTP/1.0"或"HTTP/1.1"</p>
     * <p>对于HTTP/2消息返回"HTTP/2"</p>
     * @return 协议版本字符串
     */
    String httpVersion();

    /**
     * 获取消息正文起始偏移量
     * @return 正文起始位置（字节偏移量）
     */
    int bodyOffset();

    /**
     * 获取消息正文字节数组
     * @return 正文字节数组
     */
    ByteArray body();

    /**
     * 获取消息正文字符串表示
     * @return 正文内容字符串
     */
    String bodyToString();

    /**
     * 获取消息标记列表
     * <p>标记通常用于高亮显示消息中的特定部分</p>
     * @return 标记对象列表
     */
    List<Marker> markers();

    /**
     * 在消息中搜索指定文本
     * @param searchTerm 要搜索的文本
     * @param caseSensitive 是否区分大小写
     * @return 如果找到返回 true
     */
    boolean contains(String searchTerm, boolean caseSensitive);

    /**
     * 使用正则表达式搜索消息内容
     * @param pattern 正则表达式对象
     * @return 如果匹配返回 true
     */
    boolean contains(Pattern pattern);

    /**
     * 获取完整消息的字节数组表示
     * @return 包含完整消息的字节数组
     */
    ByteArray toByteArray();

    /**
     * 获取完整消息的字符串表示
     * @return 消息的字符串形式
     */
    @Override
    String toString();
}
```
#### HttpRequestResponse
```java
/*
 * 版权所有 (c) 2022-2023。PortSwigger Ltd. 保留所有权利。
 *
 * 此代码可用于扩展 Burp Suite Community Edition 和 Burp Suite Professional 的功能，
 * 前提是该使用不违反这些产品的许可条款。
 */

package burp.api.montoya.http.message;

import burp.api.montoya.core.Annotations;
import burp.api.montoya.core.Marker;
import burp.api.montoya.http.HttpService;
import burp.api.montoya.http.handler.TimingData;
import burp.api.montoya.http.message.requests.HttpRequest;
import burp.api.montoya.http.message.requests.MalformedRequestException;
import burp.api.montoya.http.message.responses.HttpResponse;

import java.util.List;
import java.util.Optional;
import java.util.regex.Pattern;

import static burp.api.montoya.internal.ObjectFactoryLocator.FACTORY;

/**
 * HTTP 请求-响应关联接口
 * <p>
 * 用于表示HTTP请求与其对应的响应之间的关联关系，
 * 并提供对请求、响应及相关元数据的统一访问
 */
public interface HttpRequestResponse
{
    /**
     * 获取HTTP请求对象
     * @return HTTP请求实例
     */
    HttpRequest request();

    /**
     * 获取HTTP响应对象
     * @return HTTP响应实例，如果没有响应则返回null
     */
    HttpResponse response();

    /**
     * 获取请求对应的HTTP服务信息
     * @return HTTP服务对象
     */
    HttpService httpService();

    /**
     * 获取注解信息
     * @return 注解对象
     */
    Annotations annotations();

    /**
     * 获取请求-响应的计时数据（如果可用）
     * @return 包含计时数据的Optional对象
     */
    Optional<TimingData> timingData();

    /**
     * 获取请求的URL（已弃用）
     * @return 请求URL字符串
     * @throws MalformedRequestException 如果请求格式错误
     * @deprecated 请使用 {@link #request()} 的 url() 方法替代
     */
    @Deprecated(forRemoval = true)
    String url() throws MalformedRequestException;

    /**
     * 检查是否存在响应
     * @return 如果存在响应返回true
     */
    boolean hasResponse();

    /**
     * 获取请求的内容类型（已弃用）
     * @return 内容类型枚举
     * @deprecated 请使用 {@link #request()} 的 contentType() 方法替代
     */
    @Deprecated(forRemoval = true)
    ContentType contentType();

    /**
     * 获取响应的状态码（已弃用）
     * @return HTTP状态码，如果没有响应返回-1
     * @deprecated 请使用 {@link #response()} 的 statusCode() 方法替代
     */
    @Deprecated(forRemoval = true)
    short statusCode();

    /**
     * 获取请求标记列表
     * @return 请求标记列表
     */
    List<Marker> requestMarkers();

    /**
     * 获取响应标记列表
     * @return 响应标记列表
     */
    List<Marker> responseMarkers();

    /**
     * 在请求、响应和注释中搜索文本
     * @param searchTerm 要搜索的文本
     * @param caseSensitive 是否区分大小写
     * @return 如果找到返回true
     */
    boolean contains(String searchTerm, boolean caseSensitive);

    /**
     * 在请求、响应和注释中使用正则表达式搜索
     * @param pattern 正则表达式
     * @return 如果匹配返回true
     */
    boolean contains(Pattern pattern);

    /**
     * 创建请求-响应对象的临时文件副本
     * <p>用于将对象保存到临时文件以减少内存占用</p>
     * @return 新的HttpRequestResponse实例
     */
    HttpRequestResponse copyToTempFile();

    /**
     * 创建带有新注解的请求-响应副本
     * @param annotations 要添加的注解
     * @return 新的HttpRequestResponse实例
     */
    HttpRequestResponse withAnnotations(Annotations annotations);

    /**
     * 创建带有新请求标记的副本
     * @param requestMarkers 要添加的请求标记列表
     * @return 新的HttpRequestResponse实例
     */
    HttpRequestResponse withRequestMarkers(List<Marker> requestMarkers);

    /**
     * 创建带有新请求标记的副本
     * @param requestMarkers 要添加的请求标记数组
     * @return 新的HttpRequestResponse实例
     */
    HttpRequestResponse withRequestMarkers(Marker... requestMarkers);

    /**
     * 创建带有新响应标记的副本
     * @param responseMarkers 要添加的响应标记列表
     * @return 新的HttpRequestResponse实例
     */
    HttpRequestResponse withResponseMarkers(List<Marker> responseMarkers);

    /**
     * 创建带有新响应标记的副本
     * @param responseMarkers 要添加的响应标记数组
     * @return 新的HttpRequestResponse实例
     */
    HttpRequestResponse withResponseMarkers(Marker... responseMarkers);

    /**
     * 创建新的请求-响应关联对象
     * @param request HTTP请求
     * @param response HTTP响应
     * @return 新的HttpRequestResponse实例
     */
    static HttpRequestResponse httpRequestResponse(HttpRequest request, HttpResponse response)
    {
        return FACTORY.httpRequestResponse(request, response);
    }

    /**
     * 创建带有注解的请求-响应关联对象
     * @param httpRequest HTTP请求
     * @param httpResponse HTTP响应
     * @param annotations 注解对象
     * @return 新的HttpRequestResponse实例
     */
    static HttpRequestResponse httpRequestResponse(HttpRequest httpRequest, HttpResponse httpResponse, Annotations annotations)
    {
        return FACTORY.httpRequestResponse(httpRequest, httpResponse, annotations);
    }
}
```
#### MimeType
```java
/*
 * 版权所有 (c) 2022-2023。PortSwigger Ltd. 保留所有权利。
 *
 * 此代码可用于扩展 Burp Suite Community Edition 和 Burp Suite Professional 的功能，
 * 前提是该使用不违反这些产品的许可条款。
 */

package burp.api.montoya.http.message;

/**
 * Burp 识别的 MIME 类型枚举
 * <p>
 * 表示 HTTP 消息中常见的内容类型(MIME类型)分类，
 * 用于内容类型识别和分类处理
 */
public enum MimeType
{
    NONE("none"),                          // 无内容类型
    UNRECOGNIZED("unrecognized content"),   // 无法识别的内容类型
    AMBIGUOUS("ambiguous"),                 // 模糊/不确定的内容类型
    HTML("HTML"),                           // HTML 文档
    PLAIN_TEXT("plain text"),               // 纯文本
    CSS("CSS"),                             // CSS 样式表
    SCRIPT("script"),                       // 脚本文件(如 JavaScript)
    JSON("JSON"),                           // JSON 数据
    RTF("RTF"),                             // 富文本格式
    XML("XML"),                             // XML 文档
    YAML("YAML"),                           // YAML 数据
    IMAGE_UNKNOWN("an unknown image type"), // 未知图像类型
    IMAGE_JPEG("a JPEG image"),             // JPEG 图像
    IMAGE_GIF("a GIF image"),               // GIF 图像
    IMAGE_PNG("a PNG image"),               // PNG 图像
    IMAGE_BMP("a BMP image"),               // BMP 图像
    IMAGE_TIFF("a TIFF image"),             // TIFF 图像
    IMAGE_SVG_XML("a SVG image"),           // SVG 矢量图像
    SOUND("sound"),                         // 音频文件
    VIDEO("video"),                         // 视频文件
    APPLICATION_FLASH("a flash object"),    // Flash 对象
    APPLICATION_UNKNOWN("an unknown application type"), // 未知应用程序类型
    FONT_WOFF("a WOFF font file"),          // WOFF 字体文件
    FONT_WOFF2("a WOFF2 font file"),        // WOFF2 字体文件
    LEGACY_SER_AMF("");                     // 遗留的 AMF 序列化格式

    private final String description;  // MIME 类型描述文本

    /**
     * 枚举构造函数
     * @param description MIME 类型描述文本
     */
    MimeType(String description)
    {
        this.description = description;
    }

    /**
     * 获取 MIME 类型的描述文本
     * @return 描述字符串
     */
    public String description()
    {
        return description;
    }
}
```
#### StatusCodeClass
```java
package burp.api.montoya.http.message;

/**
 * HTTP 标准定义的状态码分类枚举
 * <p>
 * 按照 HTTP 协议规范将状态码分为五大类，
 * 每类状态码表示不同类型的服务器响应
 */
public enum StatusCodeClass
{
    /**
     * 信息响应类 (100 到 199)
     * <p>表示请求已被接收，需要继续处理</p>
     */
    CLASS_1XX_INFORMATIONAL_RESPONSE(100, 200),
    
    /**
     * 成功响应类 (200 到 299)
     * <p>表示请求已成功被服务器接收、理解并接受</p>
     */
    CLASS_2XX_SUCCESS(200, 300),
    
    /**
     * 重定向类 (300 到 399)
     * <p>表示需要客户端采取进一步操作才能完成请求</p>
     */
    CLASS_3XX_REDIRECTION(300, 400),
    
    /**
     * 客户端错误类 (400 到 499)
     * <p>表示客户端请求有错误或无法完成</p>
     */
    CLASS_4XX_CLIENT_ERRORS(400, 500),
    
    /**
     * 服务器错误类 (500 到 599)
     * <p>表示服务器处理请求时发生错误</p>
     */
    CLASS_5XX_SERVER_ERRORS(500, 600);

    private final int startStatusCodeInclusive;  // 状态码范围起始值(包含)
    private final int endStatusCodeExclusive;    // 状态码范围结束值(不包含)

    /**
     * 枚举构造函数
     * @param startStatusCodeInclusive 状态码起始值(包含)
     * @param endStatusCodeExclusive 状态码结束值(不包含)
     */
    StatusCodeClass(int startStatusCodeInclusive, int endStatusCodeExclusive)
    {
        this.startStatusCodeInclusive = startStatusCodeInclusive;
        this.endStatusCodeExclusive = endStatusCodeExclusive;
    }

    /**
     * 获取状态码范围的起始值(包含)
     * @return 起始状态码
     */
    public int startStatusCodeInclusive()
    {
        return startStatusCodeInclusive;
    }

    /**
     * 获取状态码范围的结束值(不包含)
     * @return 结束状态码
     */
    public int endStatusCodeExclusive()
    {
        return endStatusCodeExclusive;
    }

    /**
     * 检查指定状态码是否属于当前分类
     * @param statusCode 要检查的状态码
     * @return 如果状态码在当前分类范围内返回 true
     */
    public boolean contains(int statusCode)
    {
        return startStatusCodeInclusive <= statusCode && statusCode < endStatusCodeExclusive;
    }
}
```
### params
#### HttpParameter
```java
/*
 * 版权所有 (c) 2022-2023。PortSwigger Ltd. 保留所有权利。
 *
 * 此代码可用于扩展 Burp Suite Community Edition 和 Burp Suite Professional 的功能，
 * 前提是该使用不违反这些产品的许可条款。
 */

package burp.api.montoya.http.message.params;

import static burp.api.montoya.internal.ObjectFactoryLocator.FACTORY;

/**
 * HTTP 请求参数接口
 * <p>
 * 表示 HTTP 请求中的参数，包含参数名称、值和类型信息，
 * 支持 URL 参数、请求体参数、Cookie 参数等多种类型
 */
public interface HttpParameter
{
    /**
     * 获取参数类型
     * @return 参数类型枚举值
     */
    HttpParameterType type();

    /**
     * 获取参数名称
     * @return 参数名称字符串
     */
    String name();

    /**
     * 获取参数值
     * @return 参数值字符串
     */
    String value();

    /**
     * 创建 URL 类型参数实例
     * <p>通常用于查询字符串参数</p>
     * @param name 参数名称
     * @param value 参数值
     * @return 新的 HttpParameter 实例
     */
    static HttpParameter urlParameter(String name, String value)
    {
        return FACTORY.urlParameter(name, value);
    }

    /**
     * 创建请求体类型参数实例
     * <p>通常用于 POST 表单参数</p>
     * @param name 参数名称
     * @param value 参数值
     * @return 新的 HttpParameter 实例
     */
    static HttpParameter bodyParameter(String name, String value)
    {
        return FACTORY.bodyParameter(name, value);
    }

    /**
     * 创建 Cookie 类型参数实例
     * @param name Cookie 名称
     * @param value Cookie 值
     * @return 新的 HttpParameter 实例
     */
    static HttpParameter cookieParameter(String name, String value)
    {
        return FACTORY.cookieParameter(name, value);
    }

    /**
     * 创建指定类型的参数实例
     * @param name 参数名称
     * @param value 参数值
     * @param type 参数类型
     * @return 新的 HttpParameter 实例
     */
    static HttpParameter parameter(String name, String value, HttpParameterType type)
    {
        return FACTORY.parameter(name, value, type);
    }
}
```
#### HttpParameterType
```java
/*
 * 版权所有 (c) 2022-2023。PortSwigger Ltd. 保留所有权利。
 *
 * 此代码可用于扩展 Burp Suite Community Edition 和 Burp Suite Professional 的功能，
 * 前提是该使用不违反这些产品的许可条款。
 */

package burp.api.montoya.http.message.params;

/**
 * HTTP 参数类型枚举
 * <p>
 * 定义 HTTP 请求中不同类型的参数位置和格式，
 * 用于精确识别和操作请求中的各种参数
 */
public enum HttpParameterType
{
    /**
     * URL 查询参数 - 出现在问号(?)后的查询字符串中
     * <p>示例: example.com?name=value</p>
     */
    URL,

    /**
     * 请求体参数 - 出现在 POST 请求体中
     * <p>通常以 application/x-www-form-urlencoded 格式传输</p>
     */
    BODY,

    /**
     * Cookie 参数 - 出现在 HTTP Cookie 头部中
     * <p>示例: Cookie: name=value</p>
     */
    COOKIE,

    /**
     * XML 元素参数 - 出现在 XML 请求体中的元素
     * <p>示例: &lt;name&gt;value&lt;/name&gt;</p>
     */
    XML,

    /**
     * XML 属性参数 - 出现在 XML 元素的属性中
     * <p>示例: &lt;element name="value"&gt;</p>
     */
    XML_ATTRIBUTE,

    /**
     * 多部分表单属性 - 出现在 multipart/form-data 请求中
     * <p>用于文件上传等场景</p>
     */
    MULTIPART_ATTRIBUTE,

    /**
     * JSON 参数 - 出现在 JSON 格式的请求体中
     * <p>示例: {"name": "value"}</p>
     */
    JSON
}
```
#### ParsedHttpParameter
```java
/*
 * 版权所有 (c) 2022-2023。PortSwigger Ltd. 保留所有权利。
 *
 * 此代码可用于扩展 Burp Suite Community Edition 和 Burp Suite Professional 的功能，
 * 前提是该使用不违反这些产品的许可条款。
 */

package burp.api.montoya.http.message.params;

import burp.api.montoya.core.Range;

/**
 * 解析后的HTTP参数接口
 * <p>
 * 继承自 {@link HttpParameter}，增加了参数在请求中的位置信息，
 * 表示已被Burp解析的HTTP请求参数，包含参数名和值的具体偏移位置
 */
public interface ParsedHttpParameter extends HttpParameter
{
    /**
     * 获取参数类型
     * @return 参数类型枚举值
     */
    @Override
    HttpParameterType type();

    /**
     * 获取参数名称
     * @return 参数名称字符串
     */
    @Override
    String name();

    /**
     * 获取参数值
     * @return 参数值字符串
     */
    @Override
    String value();

    /**
     * 获取参数名称在HTTP请求中的偏移范围
     * <p>用于定位参数名在原始请求中的具体位置</p>
     * @return 表示名称位置的Range对象
     */
    Range nameOffsets();

    /**
     * 获取参数值在HTTP请求中的偏移范围
     * <p>用于定位参数值在原始请求中的具体位置</p>
     * @return 表示值位置的Range对象
     */
    Range valueOffsets();
}
```
### requests
#### HttpRequest
```java
/*
 * 版权所有 (c) 2022-2023。PortSwigger Ltd. 保留所有权利。
 *
 * 此代码可用于扩展 Burp Suite Community Edition 和 Burp Suite Professional 的功能，
 * 前提是该使用不违反这些产品的许可条款。
 */

package burp.api.montoya.http.message.requests;

import burp.api.montoya.core.ByteArray;
import burp.api.montoya.core.Marker;
import burp.api.montoya.http.HttpService;
import burp.api.montoya.http.message.ContentType;
import burp.api.montoya.http.message.HttpHeader;
import burp.api.montoya.http.message.HttpMessage;
import burp.api.montoya.http.message.params.HttpParameter;
import burp.api.montoya.http.message.params.HttpParameterType;
import burp.api.montoya.http.message.params.ParsedHttpParameter;

import java.util.List;
import java.util.regex.Pattern;

import static burp.api.montoya.internal.ObjectFactoryLocator.FACTORY;

/**
 * HTTP 请求接口
 * <p>
 * 表示一个完整的 HTTP 请求消息，提供对请求各个部分的访问和修改方法，
 * 继承自 {@link HttpMessage} 并扩展了请求特有的功能
 */
public interface HttpRequest extends HttpMessage
{
    /**
     * 检查请求是否在目标范围内
     * @return 如果在范围内返回 true
     */
    boolean isInScope();

    /**
     * 获取请求对应的 HTTP 服务信息
     * @return HTTP 服务对象
     */
    HttpService httpService();

    /**
     * 获取请求的完整 URL
     * @return URL 字符串
     * @throws MalformedRequestException 如果请求格式错误
     */
    String url() throws MalformedRequestException;

    /**
     * 获取请求的 HTTP 方法
     * @return HTTP 方法（如 GET/POST 等）
     * @throws MalformedRequestException 如果请求格式错误
     */
    String method() throws MalformedRequestException;

    /**
     * 获取请求路径（包含查询参数）
     * @return 路径字符串
     * @throws MalformedRequestException 如果请求格式错误
     */
    String path() throws MalformedRequestException;

    /**
     * 获取请求的查询字符串
     * @return 查询字符串，如果没有则返回空字符串
     * @throws MalformedRequestException 如果请求格式错误
     */
    String query() throws MalformedRequestException;

    /**
     * 获取请求路径（不包含查询参数）
     * @return 路径字符串
     * @throws MalformedRequestException 如果请求格式错误
     */
    String pathWithoutQuery() throws MalformedRequestException;

    /**
     * 获取请求的文件扩展名
     * @return 文件扩展名，如果没有则返回空字符串
     * @throws MalformedRequestException 如果请求格式错误
     */
    String fileExtension() throws MalformedRequestException;

    /**
     * 获取请求的内容类型
     * @return 内容类型枚举
     */
    ContentType contentType();

    /**
     * 获取请求中的所有参数
     * @return 解析后的参数列表
     */
    List<ParsedHttpParameter> parameters();

    /**
     * 获取指定类型的参数列表
     * @param type 参数类型
     * @return 过滤后的参数列表
     */
    List<ParsedHttpParameter> parameters(HttpParameterType type);

    /**
     * 检查请求是否包含参数
     * @return 如果包含参数返回 true
     */
    boolean hasParameters();

    /**
     * 检查请求是否包含指定类型的参数
     * @param type 参数类型
     * @return 如果包含返回 true
     */
    boolean hasParameters(HttpParameterType type);

    /**
     * 获取指定名称和类型的参数
     * @param name 参数名
     * @param type 参数类型
     * @return 参数对象，如果不存在返回 null
     */
    ParsedHttpParameter parameter(String name, HttpParameterType type);

    /**
     * 获取指定名称和类型参数的值
     * @param name 参数名
     * @param type 参数类型
     * @return 参数值，如果不存在返回 null
     */
    String parameterValue(String name, HttpParameterType type);

    /**
     * 获取指定名称的参数（不限类型）
     * @param name 参数名
     * @return 参数对象，如果不存在返回 null
     */
    ParsedHttpParameter parameter(String name);

    /**
     * 获取指定名称参数的值（不限类型）
     * @param name 参数名
     * @return 参数值，如果不存在返回 null
     */
    String parameterValue(String name);

    /**
     * 检查是否存在指定名称和类型的参数
     * @param name 参数名
     * @param type 参数类型
     * @return 如果存在返回 true
     */
    boolean hasParameter(String name, HttpParameterType type);

    /**
     * 检查是否存在与给定参数匹配的参数
     * @param parameter 要匹配的参数
     * @return 如果存在返回 true
     */
    boolean hasParameter(HttpParameter parameter);

    /**
     * 检查消息中是否包含指定头部
     * @param header 要检查的头部对象
     * @return 如果存在返回 true
     */
    @Override
    boolean hasHeader(HttpHeader header);

    /**
     * 检查消息中是否包含指定名称的头部
     * @param name 头部名称
     * @return 如果存在返回 true
     */
    @Override
    boolean hasHeader(String name);

    /**
     * 检查消息中是否包含指定名称和值的头部
     * @param name 头部名称
     * @param value 头部值
     * @return 如果匹配返回 true
     */
    @Override
    boolean hasHeader(String name, String value);

    /**
     * 获取指定名称的头部对象
     * @param name 头部名称
     * @return 头部对象，如果不存在返回 null
     */
    @Override
    HttpHeader header(String name);

    /**
     * 获取指定名称头部的值
     * @param name 头部名称
     * @return 头部值字符串，如果不存在返回 null
     */
    @Override
    String headerValue(String name);

    /**
     * 获取消息中的所有头部列表
     * @return HTTP 头部列表
     */
    @Override
    List<HttpHeader> headers();

    /**
     * 获取HTTP协议版本
     * <p>对于HTTP/1.x消息返回"HTTP/1.0"或"HTTP/1.1"</p>
     * <p>对于HTTP/2消息返回"HTTP/2"</p>
     * @return 协议版本字符串
     */
    @Override
    String httpVersion();

    /**
     * 获取消息正文起始偏移量
     * @return 正文起始位置（字节偏移量）
     */
    @Override
    int bodyOffset();

    /**
     * 获取消息正文字节数组
     * @return 正文字节数组
     */
    @Override
    ByteArray body();

    /**
     * 获取消息正文字符串表示
     * @return 正文内容字符串
     */
    @Override
    String bodyToString();

    /**
     * 获取消息标记列表
     * <p>标记通常用于高亮显示消息中的特定部分</p>
     * @return 标记对象列表
     */
    @Override
    List<Marker> markers();

    /**
     * 在消息中搜索指定文本
     * @param searchTerm 要搜索的文本
     * @param caseSensitive 是否区分大小写
     * @return 如果找到返回 true
     */
    @Override
    boolean contains(String searchTerm, boolean caseSensitive);

    /**
     * 使用正则表达式搜索消息内容
     * @param pattern 正则表达式对象
     * @return 如果匹配返回 true
     */
    @Override
    boolean contains(Pattern pattern);

    /**
     * 获取完整消息的字节数组表示
     * @return 包含完整消息的字节数组
     */
    @Override
    ByteArray toByteArray();

    /**
     * 获取完整消息的字符串表示
     * @return 消息的字符串形式
     */
    @Override
    String toString();

    /**
     * 创建请求的临时文件副本
     * <p>用于将请求保存到临时文件以减少内存占用</p>
     * @return 新的 HttpRequest 实例
     */
    HttpRequest copyToTempFile();

    /**
     * 创建使用新服务的请求副本
     * @param service HTTP 服务对象
     * @return 新的 HttpRequest 实例
     */
    HttpRequest withService(HttpService service);

    /**
     * 创建使用新路径的请求副本
     * @param path 新路径
     * @return 新的 HttpRequest 实例
     */
    HttpRequest withPath(String path);

    /**
     * 创建使用新方法的请求副本
     * @param method 新方法
     * @return 新的 HttpRequest 实例
     */
    HttpRequest withMethod(String method);

    /**
     * 创建添加/更新头后的请求副本
     * @param header HTTP 头对象
     * @return 新的 HttpRequest 实例
     */
    HttpRequest withHeader(HttpHeader header);

    /**
     * 创建添加/更新头后的请求副本
     * @param name 头名称
     * @param value 头值
     * @return 新的 HttpRequest 实例
     */
    HttpRequest withHeader(String name, String value);

    /**
     * 创建添加/更新参数后的请求副本
     * @param parameters HTTP 参数
     * @return 新的 HttpRequest 实例
     */
    HttpRequest withParameter(HttpParameter parameters);

    /**
     * 创建添加多个参数后的请求副本
     * @param parameters HTTP 参数列表
     * @return 新的 HttpRequest 实例
     */
    HttpRequest withAddedParameters(List<? extends HttpParameter> parameters);

    /**
     * 创建添加多个参数后的请求副本
     * @param parameters HTTP 参数数组
     * @return 新的 HttpRequest 实例
     */
    HttpRequest withAddedParameters(HttpParameter... parameters);

    /**
     * 创建移除多个参数后的请求副本
     * @param parameters 要移除的参数列表
     * @return 新的 HttpRequest 实例
     */
    HttpRequest withRemovedParameters(List<? extends HttpParameter> parameters);

    /**
     * 创建移除多个参数后的请求副本
     * @param parameters 要移除的参数数组
     * @return 新的 HttpRequest 实例
     */
    HttpRequest withRemovedParameters(HttpParameter... parameters);

    /**
     * 创建更新多个参数后的请求副本
     * @param parameters 要更新的参数列表
     * @return 新的 HttpRequest 实例
     */
    HttpRequest withUpdatedParameters(List<? extends HttpParameter> parameters);

    /**
     * 创建更新多个参数后的请求副本
     * @param parameters 要更新的参数数组
     * @return 新的 HttpRequest 实例
     */
    HttpRequest withUpdatedParameters(HttpParameter... parameters);

    /**
     * 创建应用转换后的请求副本
     * @param transformation 转换对象
     * @return 新的 HttpRequest 实例
     */
    HttpRequest withTransformationApplied(HttpTransformation transformation);

    /**
     * 创建更新消息体后的请求副本
     * @param body 新消息体字符串
     * @return 新的 HttpRequest 实例
     */
    HttpRequest withBody(String body);

    /**
     * 创建更新消息体后的请求副本
     * @param body 新消息体字节数组
     * @return 新的 HttpRequest 实例
     */
    HttpRequest withBody(ByteArray body);

    /**
     * 创建添加头后的请求副本
     * @param name 头名称
     * @param value 头值
     * @return 新的 HttpRequest 实例
     */
    HttpRequest withAddedHeader(String name, String value);

    /**
     * 创建添加头后的请求副本
     * @param header HTTP 头对象
     * @return 新的 HttpRequest 实例
     */
    HttpRequest withAddedHeader(HttpHeader header);

    /**
     * 创建添加多个头后的请求副本
     * @param headers HTTP 头列表
     * @return 新的 HttpRequest 实例
     */
    HttpRequest withAddedHeaders(List<? extends HttpHeader> headers);

    /**
     * 创建添加多个头后的请求副本
     * @param headers HTTP 头数组
     * @return 新的 HttpRequest 实例
     */
    HttpRequest withAddedHeaders(HttpHeader... headers);

    /**
     * 创建更新头后的请求副本
     * @param name 要更新的头名称
     * @param value 新头值
     * @return 新的 HttpRequest 实例
     */
    HttpRequest withUpdatedHeader(String name, String value);

    /**
     * 创建更新头后的请求副本
     * @param header 包含新值的 HTTP 头对象
     * @return 新的 HttpRequest 实例
     */
    HttpRequest withUpdatedHeader(HttpHeader header);

    /**
     * 创建更新多个头后的请求副本
     * @param headers HTTP 头列表
     * @return 新的 HttpRequest 实例
     */
    HttpRequest withUpdatedHeaders(List<? extends HttpHeader> headers);

    /**
     * 创建更新多个头后的请求副本
     * @param headers HTTP 头数组
     * @return 新的 HttpRequest 实例
     */
    HttpRequest withUpdatedHeaders(HttpHeader... headers);

    /**
     * 创建移除头后的请求副本
     * @param name 要移除的头名称
     * @return 新的 HttpRequest 实例
     */
    HttpRequest withRemovedHeader(String name);

    /**
     * 创建移除头后的请求副本
     * @param header 要移除的 HTTP 头对象
     * @return 新的 HttpRequest 实例
     */
    HttpRequest withRemovedHeader(HttpHeader header);

    /**
     * 创建移除多个头后的请求副本
     * @param headers HTTP 头列表
     * @return 新的 HttpRequest 实例
     */
    HttpRequest withRemovedHeaders(List<? extends HttpHeader> headers);

    /**
     * 创建移除多个头后的请求副本
     * @param headers HTTP 头数组
     * @return 新的 HttpRequest 实例
     */
    HttpRequest withRemovedHeaders(HttpHeader... headers);

    /**
     * 创建添加标记后的请求副本
     * @param markers 标记列表
     * @return 新的 HttpRequest 实例
     */
    HttpRequest withMarkers(List<Marker> markers);

    /**
     * 创建添加标记后的请求副本
     * @param markers 标记数组
     * @return 新的 HttpRequest 实例
     */
    HttpRequest withMarkers(Marker... markers);

    /**
     * 创建添加默认头后的请求副本
     * @return 新的 HttpRequest 实例
     */
    HttpRequest withDefaultHeaders();

    /**
     * 创建新的空 HTTP 请求实例
     * @return 新的 HttpRequest 实例
     */
    static HttpRequest httpRequest()
    {
        return FACTORY.httpRequest();
    }

    /**
     * 从字节数组创建 HTTP 请求实例
     * @param request 请求字节数组
     * @return 新的 HttpRequest 实例
     */
    static HttpRequest httpRequest(ByteArray request)
    {
        return FACTORY.httpRequest(request);
    }

    /**
     * 从字符串创建 HTTP 请求实例
     * @param request 请求字符串
     * @return 新的 HttpRequest 实例
     */
    static HttpRequest httpRequest(String request)
    {
        return FACTORY.httpRequest(request);
    }

    /**
     * 从服务信息和字节数组创建 HTTP 请求实例
     * @param service HTTP 服务信息
     * @param request 请求字节数组
     * @return 新的 HttpRequest 实例
     */
    static HttpRequest httpRequest(HttpService service, ByteArray request)
    {
        return FACTORY.httpRequest(service, request);
    }

    /**
     * 从服务信息和字符串创建 HTTP 请求实例
     * @param service HTTP 服务信息
     * @param request 请求字符串
     * @return 新的 HttpRequest 实例
     */
    static HttpRequest httpRequest(HttpService service, String request)
    {
        return FACTORY.httpRequest(service, request);
    }

    /**
     * 从 URL 创建 HTTP 请求实例
     * @param url 请求 URL
     * @return 新的 HttpRequest 实例
     */
    static HttpRequest httpRequestFromUrl(String url)
    {
        return FACTORY.httpRequestFromUrl(url);
    }

    /**
     * 创建 HTTP/2 请求实例
     * @param service HTTP 服务信息
     * @param headers HTTP/2 头部列表
     * @param body 请求体字节数组
     * @return 新的 HttpRequest 实例
     */
    static HttpRequest http2Request(HttpService service, List<HttpHeader> headers, ByteArray body)
    {
        return FACTORY.http2Request(service, headers, body);
    }

    /**
     * 创建 HTTP/2 请求实例
     * @param service HTTP 服务信息
     * @param headers HTTP/2 头部列表
     * @param body 请求体字符串
     * @return 新的 HttpRequest 实例
     */
    static HttpRequest http2Request(HttpService service, List<HttpHeader> headers, String body)
    {
        return FACTORY.http2Request(service, headers, body);
    }
}
```
#### HttpTransformation
```java
/*
 * 版权所有 (c) 2022-2023。PortSwigger Ltd. 保留所有权利。
 *
 * 此代码可用于扩展 Burp Suite Community Edition 和 Burp Suite Professional 的功能，
 * 前提是该使用不违反这些产品的许可条款。
 */

package burp.api.montoya.http.message.requests;

/**
 * HTTP 请求转换类型枚举
 * <p>
 * 定义 Burp 可以对 HTTP 请求应用的各种转换操作，
 * 用于修改请求的基本结构和属性
 */
public enum HttpTransformation
{
    /**
     * HTTP 方法切换转换
     * <p>
     * 将 GET 请求转换为 POST 请求<br>
     * 或<br>
     * 将 POST 请求转换为 GET 请求<br>
     * 
     * <p>转换时会自动处理以下内容：</p>
     * <ul>
     *   <li>GET 转 POST 时，原查询参数会移动到请求体中</li>
     *   <li>POST 转 GET 时，请求体参数会移动到 URL 查询字符串中</li>
     *   <li>自动更新 Content-Length 等必要头部</li>
     * </ul>
     */
    TOGGLE_METHOD
}
```
#### MalformedRequestException
```java
/*
 * 版权所有 (c) 2022-2023。PortSwigger Ltd. 保留所有权利。
 *
 * 此代码可用于扩展 Burp Suite Community Edition 和 Burp Suite Professional 的功能，
 * 前提是该使用不违反这些产品的许可条款。
 */

package burp.api.montoya.http.message.requests;

/**
 * 畸形请求异常类
 * <p>
 * 当尝试从格式错误的 HTTP 请求中获取属性时抛出此异常，
 * 表示请求格式不符合 HTTP 协议规范，无法正常解析
 */
public class MalformedRequestException extends RuntimeException
{
    /**
     * 构造畸形请求异常实例
     * @param message 异常详细信息，描述请求格式错误的具体原因
     */
    public MalformedRequestException(String message)
    {
        super(message);
    }
}
```
### sessions
#### ActionResult
```java
/*
 * 版权所有 (c) 2022-2023。PortSwigger Ltd. 保留所有权利。
 *
 * 此代码可用于扩展 Burp Suite Community Edition 和 Burp Suite Professional 的功能，
 * 前提是该使用不违反这些产品的许可条款。
 */

package burp.api.montoya.http.sessions;

import burp.api.montoya.core.Annotations;
import burp.api.montoya.http.message.requests.HttpRequest;

import static burp.api.montoya.internal.ObjectFactoryLocator.FACTORY;

/**
 * 会话处理动作结果接口
 * <p>
 * 表示 {@link SessionHandlingAction#performAction(SessionHandlingActionData)} 方法的返回结果，
 * 包含处理后的HTTP请求和可选的注解信息
 */
public interface ActionResult
{
    /**
     * 获取处理后的HTTP请求
     * @return 处理后的HTTP请求对象
     */
    HttpRequest request();

    /**
     * 获取关联的注解信息
     * @return 注解对象，可能包含会话处理过程中的额外信息
     */
    Annotations annotations();

    /**
     * 创建新的动作结果实例（不修改注解）
     *
     * @param request 处理后的HTTP请求
     * @return 新的ActionResult实例
     */
    static ActionResult actionResult(HttpRequest request)
    {
        return FACTORY.actionResult(request);
    }

    /**
     * 创建新的动作结果实例（包含修改后的注解）
     *
     * @param request 处理后的HTTP请求
     * @param annotations 修改后的注解对象
     * @return 新的ActionResult实例
     */
    static ActionResult actionResult(HttpRequest request, Annotations annotations)
    {
        return FACTORY.actionResult(request, annotations);
    }
}
```
#### CookieJar
```java
/*
 * 版权所有 (c) 2022-2023。PortSwigger Ltd. 保留所有权利。
 *
 * 此代码可用于扩展 Burp Suite Community Edition 和 Burp Suite Professional 的功能，
 * 前提是该使用不违反这些产品的许可条款。
 */

package burp.api.montoya.http.sessions;

import burp.api.montoya.http.message.Cookie;

import java.time.ZonedDateTime;
import java.util.List;

/**
 * Cookie 存储管理接口
 * <p>
 * 提供对 Burp Cookie Jar 功能的访问，允许添加和获取 HTTP Cookie
 */
public interface CookieJar
{
    /**
     * 添加新的 HTTP Cookie 到 Cookie Jar
     *
     * @param name       Cookie 名称
     * @param value      Cookie 值
     * @param path       Cookie 的作用域路径，如果没有设置则为 {@code null}
     * @param domain     Cookie 的作用域域名
     * @param expiration Cookie 的过期时间，如果是会话 Cookie 则为 {@code null}
     */
    void setCookie(String name, String value, String path, String domain, ZonedDateTime expiration);

    /**
     * 获取 Cookie Jar 中存储的所有 Cookie
     *
     * @return Cookie 列表，包含所有存储的 Cookie 信息
     */
    List<Cookie> cookies();
}
```
#### SessionHandlingAction
```java
/*
 * 版权所有 (c) 2022-2023。PortSwigger Ltd. 保留所有权利。
 *
 * 此代码可用于扩展 Burp Suite Community Edition 和 Burp Suite Professional 的功能，
 * 前提是该使用不违反这些产品的许可条款。
 */

package burp.api.montoya.http.sessions;

import burp.api.montoya.http.Http;

/**
 * 会话处理动作接口
 * <p>
 * 扩展程序实现此接口并通过 {@link Http#registerSessionHandlingAction} 注册自定义会话处理动作。
 * 每个注册的动作将会出现在会话处理规则UI中供用户选择。
 * 用户可以选择直接执行动作，或在宏执行后执行。
 */
public interface SessionHandlingAction
{
    /**
     * 获取动作名称
     * <p>该名称将显示在Burp的会话处理规则配置界面中</p>
     * @return 动作名称字符串
     */
    String name();

    /**
     * 执行会话处理动作
     * <p>
     * 当会话处理动作需要执行时调用，可能是作为独立动作执行，
     * 也可能是作为宏执行后的子动作执行。<br>
     * 实现中可以发送额外的请求，并通过 {@link ActionResult} 返回修改后的基础请求。
     *
     * @param actionData 会话处理动作数据对象，可查询基础请求的详细信息
     * @return 包含处理结果的 {@link ActionResult} 实例
     */
    ActionResult performAction(SessionHandlingActionData actionData);
}
```
#### SessionHandlingActionData
```java
/*
 * 版权所有 (c) 2022-2023。PortSwigger Ltd. 保留所有权利。
 *
 * 此代码可用于扩展 Burp Suite Community Edition 和 Burp Suite Professional 的功能，
 * 前提是该使用不违反这些产品的许可条款。
 */

package burp.api.montoya.http.sessions;

import burp.api.montoya.core.Annotations;
import burp.api.montoya.http.message.HttpRequestResponse;
import burp.api.montoya.http.message.requests.HttpRequest;

import java.util.List;

/**
 * 会话处理动作数据接口
 * <p>
 * 提供会话处理过程中所需的上下文信息，
 * 包括基础请求、宏执行结果和请求注解等
 */
public interface SessionHandlingActionData
{
    /**
     * 获取当前正在处理的基础请求
     * @return 基础HTTP请求对象
     */
    HttpRequest request();

    /**
     * 获取宏执行结果
     * <p>
     * 如果当前动作是在宏执行后被调用，
     * 则返回宏执行过程中生成的请求/响应列表。<br>
     * 如果动作是直接调用，则返回空列表。<br>
     * 可用于分析宏执行结果，提取非标准会话令牌等。
     *
     * @return 宏执行生成的请求/响应列表，非宏调用时返回空列表
     */
    List<HttpRequestResponse> macroRequestResponses();

    /**
     * 获取请求的注解信息
     * @return 包含请求注解的对象
     */
    Annotations annotations();
}
```
## internal
### MontoyaObjectFactory
```java
/*
 * 版权所有 (c) 2022-2023。PortSwigger Ltd. 保留所有权利。
 *
 * 此代码可用于扩展 Burp Suite Community Edition 和 Burp Suite Professional 的功能，
 * 前提是该使用不违反这些产品的许可条款。
 */

package burp.api.montoya.internal;

// 导入所有必要的接口（保持原样）
import burp.api.montoya.ai.chat.Message;
import burp.api.montoya.ai.chat.PromptOptions;
import burp.api.montoya.collaborator.InteractionFilter;
import burp.api.montoya.collaborator.SecretKey;
import burp.api.montoya.core.Annotations;
import burp.api.montoya.core.ByteArray;
import burp.api.montoya.core.HighlightColor;
import burp.api.montoya.core.Marker;
import burp.api.montoya.core.Range;
import burp.api.montoya.http.HttpService;
import burp.api.montoya.http.RequestOptions;
import burp.api.montoya.http.handler.RequestToBeSentAction;
import burp.api.montoya.http.handler.ResponseReceivedAction;
import burp.api.montoya.http.message.HttpHeader;
import burp.api.montoya.http.message.HttpRequestResponse;
import burp.api.montoya.http.message.params.HttpParameter;
import burp.api.montoya.http.message.params.HttpParameterType;
import burp.api.montoya.http.message.requests.HttpRequest;
import burp.api.montoya.http.message.responses.HttpResponse;
import burp.api.montoya.http.sessions.ActionResult;
import burp.api.montoya.intruder.GeneratedPayload;
import burp.api.montoya.intruder.HttpRequestTemplate;
import burp.api.montoya.intruder.HttpRequestTemplateGenerationOptions;
import burp.api.montoya.intruder.PayloadProcessingAction;
import burp.api.montoya.intruder.PayloadProcessingResult;
import burp.api.montoya.persistence.PersistedList;
import burp.api.montoya.persistence.PersistedObject;
import burp.api.montoya.proxy.MessageReceivedAction;
import burp.api.montoya.proxy.MessageToBeSentAction;
import burp.api.montoya.proxy.http.ProxyRequestReceivedAction;
import burp.api.montoya.proxy.http.ProxyRequestToBeSentAction;
import burp.api.montoya.proxy.http.ProxyResponseReceivedAction;
import burp.api.montoya.proxy.http.ProxyResponseToBeSentAction;
import burp.api.montoya.proxy.websocket.BinaryMessageReceivedAction;
import burp.api.montoya.proxy.websocket.BinaryMessageToBeSentAction;
import burp.api.montoya.proxy.websocket.TextMessageReceivedAction;
import burp.api.montoya.proxy.websocket.TextMessageToBeSentAction;
import burp.api.montoya.scanner.AuditConfiguration;
import burp.api.montoya.scanner.AuditResult;
import burp.api.montoya.scanner.BuiltInAuditConfiguration;
import burp.api.montoya.scanner.CrawlConfiguration;
import burp.api.montoya.scanner.audit.insertionpoint.AuditInsertionPoint;
import burp.api.montoya.scanner.audit.issues.AuditIssue;
import burp.api.montoya.scanner.audit.issues.AuditIssueConfidence;
import burp.api.montoya.scanner.audit.issues.AuditIssueDefinition;
import burp.api.montoya.scanner.audit.issues.AuditIssueSeverity;
import burp.api.montoya.sitemap.SiteMapFilter;
import burp.api.montoya.ui.Selection;
import burp.api.montoya.ui.menu.BasicMenuItem;
import burp.api.montoya.ui.menu.Menu;
import burp.api.montoya.ui.settings.SettingsPanelBuilder;
import burp.api.montoya.ui.settings.SettingsPanelSetting;
import burp.api.montoya.utilities.json.JsonArrayNode;
import burp.api.montoya.utilities.json.JsonBooleanNode;
import burp.api.montoya.utilities.json.JsonNode;
import burp.api.montoya.utilities.json.JsonNullNode;
import burp.api.montoya.utilities.json.JsonNumberNode;
import burp.api.montoya.utilities.json.JsonObjectNode;
import burp.api.montoya.utilities.json.JsonStringNode;
import burp.api.montoya.utilities.shell.ExecuteOptions;
import burp.api.montoya.websocket.BinaryMessageAction;
import burp.api.montoya.websocket.MessageAction;
import burp.api.montoya.websocket.TextMessageAction;

import java.util.List;
import java.util.Map;

/**
 * Burp Montoya 对象工厂接口
 * <p>
 * 提供创建 Burp API 中所有核心对象的工厂方法，
 * 是扩展功能与 Burp 核心交互的主要入口点。
 * 该接口包含创建 HTTP 消息、代理处理器、扫描器组件等各种对象的方法。
 */
public interface MontoyaObjectFactory {
    /* HTTP 服务相关方法 */
    /**
     * 从基础URL创建HTTP服务对象
     * @param baseUrl 基础URL（如"http://example.com"）
     * @return 配置好的HttpService实例
     */
    HttpService httpService(String baseUrl);

    /**
     * 创建指定主机和安全设置的HTTP服务
     * @param host 主机名或IP地址
     * @param secure 是否使用HTTPS
     * @return HttpService实例
     */
    HttpService httpService(String host, boolean secure);

    /**
     * 创建完整配置的HTTP服务
     * @param host 主机名或IP地址
     * @param port 端口号
     * @param secure 是否使用HTTPS
     * @return HttpService实例
     */
    HttpService httpService(String host, int port, boolean secure);

    /* HTTP 头部相关方法 */
    /**
     * 从名称和值创建HTTP头部
     * @param name 头部名称
     * @param value 头部值
     * @return HttpHeader实例
     */
    HttpHeader httpHeader(String name, String value);

    /**
     * 从完整头部字符串创建HTTP头部
     * @param header 完整头部字符串（如"Name: Value"）
     * @return 解析后的HttpHeader实例
     */
    HttpHeader httpHeader(String header);

    /* HTTP 参数相关方法 */
    /**
     * 创建指定类型的HTTP参数
     * @param name 参数名
     * @param value 参数值
     * @param type 参数类型（URL/BODY/COOKIE等）
     * @return HttpParameter实例
     */
    HttpParameter parameter(String name, String value, HttpParameterType type);

    /* HTTP 请求相关方法 - 完整保留所有重载 */
    HttpRequest httpRequest();
    HttpRequest httpRequest(ByteArray request);
    HttpRequest httpRequest(String request);
    HttpRequest httpRequest(HttpService service, ByteArray request);
    HttpRequest httpRequest(HttpService service, String request);
    HttpRequest http2Request(HttpService service, List<HttpHeader> headers, String body);
    HttpRequest http2Request(HttpService service, List<HttpHeader> headers, ByteArray body);
    HttpRequest httpRequestFromUrl(String url);

    /* HTTP 响应相关方法 */
    HttpResponse httpResponse();
    HttpResponse httpResponse(String response);
    HttpResponse httpResponse(ByteArray response);

    /* 请求-响应组合相关方法 */
    HttpRequestResponse httpRequestResponse(HttpRequest request, HttpResponse response, Annotations annotations);
    HttpRequestResponse httpRequestResponse(HttpRequest request, HttpResponse response);

    /* 范围标记相关方法 */
    Range range(int startIndexInclusive, int endIndexExclusive);

    /* 注解相关方法 - 完整保留所有重载 */
    Annotations annotations();
    Annotations annotations(String notes);
    Annotations annotations(HighlightColor highlightColor);
    Annotations annotations(String notes, HighlightColor highlightColor);

    /* 安全扫描相关方法 - 保留完整参数列表 */
    AuditInsertionPoint auditInsertionPoint(String name, HttpRequest baseRequest, int startIndexInclusive, int endIndexExclusive);
    AuditIssueDefinition auditIssueDefinition(String name, String background, String remediation, AuditIssueSeverity typicalSeverity);
    
    // 完整保留所有auditIssue方法签名
    AuditIssue auditIssue(
            String name,
            String detail,
            String remediation,
            String baseUrl,
            AuditIssueSeverity severity,
            AuditIssueConfidence confidence,
            String background,
            String remediationBackground,
            AuditIssueSeverity typicalSeverity,
            List<HttpRequestResponse> requestResponses);

    AuditIssue auditIssue(
            String name,
            String detail,
            String remediation,
            String baseUrl,
            AuditIssueSeverity severity,
            AuditIssueConfidence confidence,
            String background,
            String remediationBackground,
            AuditIssueSeverity typicalSeverity,
            HttpRequestResponse... requestResponses);

    /* 选择区域相关方法 */
    Selection selection(ByteArray selectionContents);
    Selection selection(int startIndexInclusive, int endIndexExclusive);
    Selection selection(ByteArray selectionContents, int startIndexInclusive, int endIndexExclusive);

    /* Collaborator相关方法 */
    SecretKey secretKey(String encodedKey);

    /* 代理相关方法 - 完整保留所有方法 */
    ProxyRequestReceivedAction proxyRequestReceivedAction(HttpRequest request, Annotations annotations, MessageReceivedAction action);
    ProxyRequestToBeSentAction proxyRequestToBeSentAction(HttpRequest request, Annotations annotations, MessageToBeSentAction action);
    ProxyResponseToBeSentAction proxyResponseToReturnAction(HttpResponse response, Annotations annotations, MessageToBeSentAction action);
    ProxyResponseReceivedAction proxyResponseReceivedAction(HttpResponse response, Annotations annotations, MessageReceivedAction action);

    /* HTTP处理器相关方法 */
    RequestToBeSentAction requestResult(HttpRequest request, Annotations annotations);
    ResponseReceivedAction responseResult(HttpResponse response, Annotations annotations);

    /* 入侵者相关方法 */
    HttpRequestTemplate httpRequestTemplate(ByteArray content, List<Range> insertionPointOffsets);
    HttpRequestTemplate httpRequestTemplate(HttpRequest request, List<Range> insertionPointOffsets);
    HttpRequestTemplate httpRequestTemplate(ByteArray content, HttpRequestTemplateGenerationOptions options);
    HttpRequestTemplate httpRequestTemplate(HttpRequest request, HttpRequestTemplateGenerationOptions options);
    PayloadProcessingResult payloadProcessingResult(ByteArray processedPayload, PayloadProcessingAction action);

    /* 交互过滤相关方法 */
    InteractionFilter interactionIdFilter(String id);
    InteractionFilter interactionPayloadFilter(String payload);

    /* 站点地图过滤 */
    SiteMapFilter prefixFilter(String prefix);

    /* 标记相关方法 */
    Marker marker(Range range);
    Marker marker(int startIndexInclusive, int endIndexExclusive);

    /* 字节数组相关方法 - 完整保留所有重载 */
    ByteArray byteArrayOfLength(int length);
    ByteArray byteArray(byte[] bytes);
    ByteArray byteArray(int[] ints);
    ByteArray byteArray(String text);

    /* WebSocket相关方法 - 完整保留所有方法 */
    TextMessageAction continueWithTextMessage(String payload);
    TextMessageAction dropTextMessage();
    TextMessageAction textMessageAction(String payload, MessageAction action);
    BinaryMessageAction continueWithBinaryMessage(ByteArray payload);
    BinaryMessageAction dropBinaryMessage();
    BinaryMessageAction binaryMessageAction(ByteArray payload, MessageAction action);
    BinaryMessageReceivedAction followUserRulesInitialProxyBinaryMessage(ByteArray payload);
    TextMessageReceivedAction followUserRulesInitialProxyTextMessage(String payload);
    BinaryMessageReceivedAction interceptInitialProxyBinaryMessage(ByteArray payload);
    TextMessageReceivedAction interceptInitialProxyTextMessage(String payload);
    BinaryMessageReceivedAction dropInitialProxyBinaryMessage();
    TextMessageReceivedAction dropInitialProxyTextMessage();
    BinaryMessageReceivedAction doNotInterceptInitialProxyBinaryMessage(ByteArray payload);
    TextMessageReceivedAction doNotInterceptInitialProxyTextMessage(String payload);
    BinaryMessageToBeSentAction continueWithFinalProxyBinaryMessage(ByteArray payload);
    TextMessageToBeSentAction continueWithFinalProxyTextMessage(String payload);
    BinaryMessageToBeSentAction dropFinalProxyBinaryMessage();
    TextMessageToBeSentAction dropFinalProxyTextMessage();

    /* 持久化相关方法 - 完整保留所有泛型方法 */
    PersistedObject persistedObject();
    PersistedList<Boolean> persistedBooleanList();
    PersistedList<Short> persistedShortList();
    PersistedList<Integer> persistedIntegerList();
    PersistedList<Long> persistedLongList();
    PersistedList<String> persistedStringList();
    PersistedList<ByteArray> persistedByteArrayList();
    PersistedList<HttpRequest> persistedHttpRequestList();
    PersistedList<HttpResponse> persistedHttpResponseList();
    PersistedList<HttpRequestResponse> persistedHttpRequestResponseList();

    /* 扫描结果相关方法 */
    AuditResult auditResult(List<AuditIssue> auditIssues);
    AuditResult auditResult(AuditIssue... auditIssues);

    /* 扫描配置相关方法 */
    AuditConfiguration auditConfiguration(BuiltInAuditConfiguration builtInAuditConfiguration);
    CrawlConfiguration crawlConfiguration(String... seedUrls);

    /* 特定参数类型方法 */
    HttpParameter urlParameter(String name, String value);
    HttpParameter bodyParameter(String name, String value);
    HttpParameter cookieParameter(String name, String value);

    /* 负载生成相关方法 */
    GeneratedPayload payload(String payload);
    GeneratedPayload payload(ByteArray payload);
    GeneratedPayload payloadEnd();
    PayloadProcessingResult usePayload(ByteArray processedPayload);
    PayloadProcessingResult skipPayload();

    /* 代理动作结果方法 - 完整保留所有重载 */
    ProxyRequestToBeSentAction requestFinalInterceptResultContinueWith(HttpRequest request);
    ProxyRequestToBeSentAction requestFinalInterceptResultContinueWith(HttpRequest request, Annotations annotations);
    ProxyRequestToBeSentAction requestFinalInterceptResultDrop();
    ProxyResponseToBeSentAction responseFinalInterceptResultDrop();
    ProxyResponseToBeSentAction responseFinalInterceptResultContinueWith(HttpResponse response, Annotations annotations);
    ProxyResponseToBeSentAction responseFinalInterceptResultContinueWith(HttpResponse response);
    ProxyResponseReceivedAction responseInitialInterceptResultIntercept(HttpResponse response);
    ProxyResponseReceivedAction responseInitialInterceptResultIntercept(HttpResponse response, Annotations annotations);
    ProxyResponseReceivedAction responseInitialInterceptResultDoNotIntercept(HttpResponse response);
    ProxyResponseReceivedAction responseInitialInterceptResultDoNotIntercept(HttpResponse response, Annotations annotations);
    ProxyResponseReceivedAction responseInitialInterceptResultFollowUserRules(HttpResponse response);
    ProxyResponseReceivedAction responseInitialInterceptResultFollowUserRules(HttpResponse response, Annotations annotations);
    ProxyResponseReceivedAction responseInitialInterceptResultDrop();
    ProxyRequestReceivedAction requestInitialInterceptResultIntercept(HttpRequest request);
    ProxyRequestReceivedAction requestInitialInterceptResultIntercept(HttpRequest request, Annotations annotations);
    ProxyRequestReceivedAction requestInitialInterceptResultDoNotIntercept(HttpRequest request);
    ProxyRequestReceivedAction requestInitialInterceptResultDoNotIntercept(HttpRequest request, Annotations annotations);
    ProxyRequestReceivedAction requestInitialInterceptResultFollowUserRules(HttpRequest request);
    ProxyRequestReceivedAction requestInitialInterceptResultFollowUserRules(HttpRequest request, Annotations annotations);
    ProxyRequestReceivedAction requestInitialInterceptResultDrop();

    /* 简化版处理器结果方法 */
    ResponseReceivedAction responseResult(HttpResponse response);
    RequestToBeSentAction requestResult(HttpRequest request);

    /* 高亮颜色方法 */
    HighlightColor highlightColor(String color);

    /* 会话处理结果方法 */
    ActionResult actionResult(HttpRequest request);
    ActionResult actionResult(HttpRequest request, Annotations annotations);

    /* 用户界面菜单方法 */
    Menu menu(String caption);
    BasicMenuItem basicMenuItem(String caption);

    /* 请求选项方法 */
    RequestOptions requestOptions();

    /* JSON处理相关方法 - 完整保留所有方法 */
    JsonNode jsonNode(String json);
    JsonArrayNode jsonArrayNode();
    JsonArrayNode jsonArrayNode(List<? extends JsonNode> value);
    JsonArrayNode jsonArrayNode(JsonNode... values);
    JsonBooleanNode jsonBooleanNode(boolean value);
    JsonNullNode jsonNullNode();
    JsonNumberNode jsonNumberNode(long value);
    JsonNumberNode jsonNumberNode(double value);
    JsonNumberNode jsonNumberNode(Number value);
    JsonObjectNode jsonObjectNode();
    JsonObjectNode jsonObjectNode(Map<String, ? extends JsonNode> value);
    JsonStringNode jsonStringNode(String value);

    /* AI聊天相关方法 */
    PromptOptions promptOptions();
    Message systemMessage(String content);
    Message userMessage(String content);
    Message assistantMessage(String content);

    /* 设置面板相关方法 - 完整保留所有重载 */
    SettingsPanelBuilder settingsPanel();
    SettingsPanelSetting integerSetting(String name);
    SettingsPanelSetting integerSetting(String name, int defaultValue);
    SettingsPanelSetting booleanSetting(String name);
    SettingsPanelSetting booleanSetting(String name, boolean defaultValue);
    SettingsPanelSetting stringSetting(String name);
    SettingsPanelSetting stringSetting(String name, String defaultValue);
    SettingsPanelSetting listSetting(String name, String... values);
    SettingsPanelSetting listSetting(String name, List<String> values, String defaultValue);

    /* 执行选项方法 */
    ExecuteOptions executeOptions();
}
```
### ObjectFactoryLocator
```java
/*
 * 版权所有 (c) 2022-2023。PortSwigger Ltd. 保留所有权利。
 *
 * 此代码可用于扩展 Burp Suite Community Edition 和 Burp Suite Professional 的功能，
 * 前提是该使用不违反这些产品的许可条款。
 */

package burp.api.montoya.internal;

/**
 * 对象工厂定位器
 * <p>
 * 提供对 Montoya 对象工厂的静态访问入口，
 * 用于获取创建 Burp 各种核心对象的工厂实例。
 * 该类的 FACTORY 字段在扩展加载时由 Burp 核心初始化。
 */
public class ObjectFactoryLocator
{
    /**
     * Montoya 对象工厂实例
     * <p>
     * 该静态字段在扩展加载时由 Burp 核心自动初始化，
     * 扩展可以通过此字段访问所有对象创建方法。
     * 
     * <p><b>注意：</b>在扩展初始化完成前访问此字段将返回 null</p>
     */
    public static MontoyaObjectFactory FACTORY = null;
}
```
## intruder
### AttackConfiguration
```java
/*
 * 版权所有 (c) 2022-2023。PortSwigger Ltd. 保留所有权利。
 *
 * 此代码可用于扩展 Burp Suite Community Edition 和 Burp Suite Professional 的功能，
 * 前提是该使用不违反这些产品的许可条款。
 */

package burp.api.montoya.intruder;

import burp.api.montoya.http.HttpService;

import java.util.Optional;

/**
 * 入侵者攻击配置接口
 * <p>
 * 表示一个入侵者攻击的配置信息，包含目标服务和请求模板等关键参数，
 * 用于定义攻击的基本行为和目标。
 */
public interface AttackConfiguration
{
    /**
     * 获取攻击目标HTTP服务信息
     * <p>
     * 当请求模板包含有效载荷标记时可能返回空Optional，
     * 表示需要从请求模板中解析目标服务。
     *
     * @return 包含HttpService的Optional对象，如果模板有载荷标记则为空
     */
    Optional<HttpService> httpService();

    /**
     * 获取HTTP请求模板
     * <p>
     * 包含原始请求内容和所有插入点偏移量信息，
     * 用于生成实际的攻击请求。
     *
     * @return HttpRequestTemplate实例，包含请求模板和插入点位置
     */
    HttpRequestTemplate requestTemplate();
}
```
### GeneratedPayload
```java
/*
 * 版权所有 (c) 2022-2023。PortSwigger Ltd. 保留所有权利。
 *
 * 此代码可用于扩展 Burp Suite Community Edition 和 Burp Suite Professional 的功能，
 * 前提是该使用不违反这些产品的许可条款。
 */

package burp.api.montoya.intruder;

import burp.api.montoya.core.ByteArray;

import static burp.api.montoya.internal.ObjectFactoryLocator.FACTORY;

/**
 * 入侵者生成的负载接口
 * <p>
 * 表示入侵者攻击中生成的一个有效负载，
 * 可以是字符串或字节数组形式，也用于标记负载生成结束。
 */
public interface GeneratedPayload
{
    /**
     * 获取负载的值
     * @return 负载内容的字节数组表示
     */
    ByteArray value();

    /**
     * 从字符串创建新的负载实例
     *
     * @param payload 字符串形式的负载值
     * @return 新的GeneratedPayload实例
     */
    static GeneratedPayload payload(String payload)
    {
        return FACTORY.payload(payload);
    }

    /**
     * 从字节数组创建新的负载实例
     *
     * @param payload 字节数组形式的负载值
     * @return 新的GeneratedPayload实例
     */
    static GeneratedPayload payload(ByteArray payload)
    {
        return FACTORY.payload(payload);
    }

    /**
     * 创建表示负载生成结束的特殊标记实例
     *
     * @return 表示结束的GeneratedPayload实例
     */
    static GeneratedPayload end()
    {
        return FACTORY.payloadEnd();
    }
}
```
### HttpRequestTemplate
```java
/*
 * 版权所有 (c) 2022-2023。PortSwigger Ltd. 保留所有权利。
 *
 * 此代码可用于扩展 Burp Suite Community Edition 和 Burp Suite Professional 的功能，
 * 前提是该使用不违反这些产品的许可条款。
 */

package burp.api.montoya.intruder;

import burp.api.montoya.core.ByteArray;
import burp.api.montoya.core.Range;
import burp.api.montoya.http.message.requests.HttpRequest;

import java.util.List;

import static burp.api.montoya.internal.ObjectFactoryLocator.FACTORY;

/**
 * 入侵者请求模板接口
 * <p>
 * 包含HTTP请求内容和插入点偏移量信息，
 * 用于定义入侵者攻击的请求结构和参数位置。
 */
public interface HttpRequestTemplate
{
    /**
     * 获取请求模板的原始内容
     * @return 请求内容的字节数组表示
     */
    ByteArray content();

    /**
     * 获取所有插入点偏移量
     * <p>
     * 每个Range对象表示一个参数在请求中的位置范围，
     * 入侵者将在这些位置插入生成的负载。
     *
     * @return 插入点偏移量Range对象列表
     */
    List<Range> insertionPointOffsets();

    /**
     * 从HttpRequest对象创建请求模板
     *
     * @param request               HTTP请求对象
     * @param insertionPointOffsets 手动指定的插入点偏移量列表
     * @return 新的请求模板实例
     */
    static HttpRequestTemplate httpRequestTemplate(HttpRequest request, List<Range> insertionPointOffsets)
    {
        return FACTORY.httpRequestTemplate(request, insertionPointOffsets);
    }

    /**
     * 从字节数组创建请求模板
     *
     * @param content               HTTP请求字节数组
     * @param insertionPointOffsets 手动指定的插入点偏移量列表
     * @return 新的请求模板实例
     */
    static HttpRequestTemplate httpRequestTemplate(ByteArray content, List<Range> insertionPointOffsets)
    {
        return FACTORY.httpRequestTemplate(content, insertionPointOffsets);
    }

    /**
     * 从HttpRequest对象自动创建请求模板
     * <p>
     * 自动在URL参数、Cookie和请求体参数位置生成插入点。
     *
     * @param request HTTP请求对象
     * @param options 模板生成选项
     * @return 新的请求模板实例
     */
    static HttpRequestTemplate httpRequestTemplate(HttpRequest request, HttpRequestTemplateGenerationOptions options)
    {
        return FACTORY.httpRequestTemplate(request, options);
    }

    /**
     * 从字节数组自动创建请求模板
     * <p>
     * 自动在URL参数、Cookie和请求体参数位置生成插入点。
     *
     * @param content HTTP请求字节数组
     * @param options 模板生成选项
     * @return 新的请求模板实例
     */
    static HttpRequestTemplate httpRequestTemplate(ByteArray content, HttpRequestTemplateGenerationOptions options)
    {
        return FACTORY.httpRequestTemplate(content, options);
    }
}
```
### HttpRequestTemplateGenerationOptions
```java
/*
 * Copyright (c) 2023. PortSwigger Ltd. All rights reserved.
 *
 * This code may be used to extend the functionality of Burp Suite Community Edition
 * and Burp Suite Professional, provided that this usage does not violate the
 * license terms for those products.
 */

package burp.api.montoya.intruder;

/**
 * 用于生成新HttpRequestTemplate的选项
 * Options that can be used to generate a new HttpRequestTemplate.
 */
public enum HttpRequestTemplateGenerationOptions
{
    /**
     * 用偏移量替换基础参数值
     * Replace base parameter value with offsets.
     */
    REPLACE_BASE_PARAMETER_VALUE_WITH_OFFSETS,

    /**
     * 将偏移量附加到基础参数值
     * Append offsets to base parameter value.
     */
    APPEND_OFFSETS_TO_BASE_PARAMETER_VALUE
}
```
### Intruder
```java
/*
 * Copyright (c) 2022-2023. PortSwigger Ltd. All rights reserved.
 *
 * 本代码可用于扩展Burp Suite社区版和Burp Suite专业版的功能，
 * 前提是该使用不违反这些产品的许可条款。
 */

package burp.api.montoya.intruder;

import burp.api.montoya.core.Registration;
import burp.api.montoya.http.HttpService;
import burp.api.montoya.http.message.requests.HttpRequest;

/**
 * 提供对Burp Intruder工具功能的访问接口
 */
public interface Intruder
{
    /**
     * 注册自定义的Intruder载荷处理器
     * <p>
     * 每个注册的处理器将在Intruder UI中可用，
     * 用户可以选择作为载荷处理规则的操作
     *
     * @param payloadProcessor 扩展程序实现的{@link PayloadProcessor}接口对象
     * @return 载荷处理器的注册句柄
     */
    Registration registerPayloadProcessor(PayloadProcessor payloadProcessor);

    /**
     * 注册Intruder载荷生成器提供者
     * <p>
     * 每个注册的提供者将在Intruder UI中可用，
     * 用户可以选择作为攻击的载荷来源
     *
     * @param payloadGeneratorProvider 扩展程序实现的PayloadGeneratorProvider接口对象
     * @return 载荷生成器提供者的注册句柄
     */
    Registration registerPayloadGeneratorProvider(PayloadGeneratorProvider payloadGeneratorProvider);

    /**
     * 发送HTTP请求到Burp Intruder工具
     * <p>
     * 请求将显示在用户界面中，攻击载荷的标记将被放置在
     * 提供的{@link HttpRequestTemplate}对象指定的位置
     *
     * @param service 指定远程服务器的主机名、端口和协议
     * @param requestTemplate 包含插入点偏移量的HTTP请求模板
     */
    void sendToIntruder(HttpService service, HttpRequestTemplate requestTemplate);

    /**
     * 发送HTTP请求到Burp Intruder工具（带命名）
     * <p>
     * 请求将显示在用户界面中，攻击载荷的标记将被放置在
     * 提供的{@link HttpRequestTemplate}对象指定的位置
     *
     * @param service 指定远程服务器的主机名、端口和协议
     * @param requestTemplate 包含插入点偏移量的HTTP请求模板
     * @param name 显示在Intruder标签页上的可选名称（为null则显示默认索引）
     */
    void sendToIntruder(HttpService service, HttpRequestTemplate requestTemplate, String name);

    /**
     * 发送HTTP请求到Burp Intruder工具
     * <p>
     * 请求将显示在用户界面中
     *
     * @param request 完整的HTTP请求
     */
    void sendToIntruder(HttpRequest request);

    /**
     * 发送HTTP请求到Burp Intruder工具（带命名）
     * <p>
     * 请求将显示在用户界面中
     *
     * @param request 完整的HTTP请求
     * @param name 显示在Intruder标签页上的名称（为null则显示默认索引）
     */
    void sendToIntruder(HttpRequest request, String name);
}
```
### IntruderInsertionPoint
```java
/*
 * Copyright (c) 2022-2023. PortSwigger Ltd. All rights reserved.
 *
 * 本代码可用于扩展Burp Suite社区版和Burp Suite专业版的功能，
 * 但不得违反相关产品的许可条款。
 */

package burp.api.montoya.intruder;

import burp.api.montoya.core.ByteArray;

/**
 * 用于攻击载荷的Intruder插入点接口
 */
public interface IntruderInsertionPoint
{
    /**
     * 获取插入点的基准值
     * 
     * @return 表示插入点基准值的字节数组
     */
    ByteArray baseValue();
}
```
### PayloadData
```java
/*
 * Copyright (c) 2022-2023. PortSwigger Ltd. All rights reserved.
 *
 * 本代码可用于扩展Burp Suite社区版和Burp Suite专业版的功能，
 * 前提是该使用不违反产品的许可条款。
 */

package burp.api.montoya.intruder;

import burp.api.montoya.core.ByteArray;

/**
 * 包含载荷数据的相关信息
 */
public interface PayloadData
{
    /**
     * 获取当前待处理的载荷值
     * 
     * @return 表示当前载荷的字节数组
     */
    ByteArray currentPayload();

    /**
     * 获取原始载荷值（在任何处理规则应用之前的值）
     * 
     * @return 表示原始载荷的字节数组 
     */
    ByteArray originalPayload();

    /**
     * 获取当前插入点数据
     * 
     * @return Intruder插入点对象
     */
    IntruderInsertionPoint insertionPoint();
}
```
###PayloadGenerator
```java
/*
 * Copyright (c) 2022-2023. PortSwigger Ltd. All rights reserved.
 *
 * 本代码可用于扩展Burp Suite社区版和专业版的功能，
 * 前提是该使用不违反产品许可条款。
 */

package burp.api.montoya.intruder;

/**
 * Intruder载荷生成器接口。
 * <p>
 * 扩展程序注册{@link PayloadGeneratorProvider}后，
 * 在进行新的Intruder攻击时需要返回此接口的新实例。
 */
public interface PayloadGenerator
{
    /**
     * 由Burp调用以获取下一个载荷值。
     * <p>
     * 当生成器完成时应返回{@link GeneratedPayload#end()}实例，
     * 向Burp发出生成结束的信号。
     *
     * @param insertionPoint 载荷的插入点信息
     * @return 生成的Intruder载荷对象
     */
    GeneratedPayload generatePayloadFor(IntruderInsertionPoint insertionPoint);
}
```
### PayloadGenerator
```java
/*
 * Copyright (c) 2022-2023. PortSwigger Ltd. All rights reserved.
 *
 * 本代码可用于扩展Burp Suite社区版和专业版的功能，
 * 前提是该使用不违反产品许可条款。
 */

package burp.api.montoya.intruder;

/**
 * Intruder载荷生成器接口。
 * <p>
 * 扩展程序在注册{@link PayloadGeneratorProvider}后，
 * 当发起新的Intruder攻击时需要实现此接口，
 * 提供载荷生成功能。
 */
public interface PayloadGenerator
{
    /**
     * 生成下一个攻击载荷。
     * <p>
     * Burp会重复调用此方法获取攻击载荷，
     * 当所有载荷生成完成后应返回{@link GeneratedPayload#end()}，
     * 通知Burp生成过程结束。
     *
     * @param insertionPoint 指定载荷插入位置的信息
     * @return 生成的攻击载荷对象
     */
    GeneratedPayload generatePayloadFor(IntruderInsertionPoint insertionPoint);
}
```
### PayloadGeneratorProvider
```java
/*
 * Copyright (c) 2022-2023. PortSwigger Ltd. All rights reserved.
 *
 * 本代码可用于扩展Burp Suite社区版和专业版的功能，
 * 前提是使用方式不违反产品许可条款。
 */

package burp.api.montoya.intruder;

/**
 * 载荷生成器提供者接口。
 * <p>
 * 扩展程序可实现此接口，并通过调用{@link Intruder#registerPayloadGeneratorProvider}
 * 来注册自定义的Intruder载荷生成器。
 */
public interface PayloadGeneratorProvider
{
    /**
     * 获取载荷生成器在UI下拉列表中显示的名称
     *
     * @return 载荷生成器的显示名称
     */
    String displayName();

    /**
     * 由Burp调用以获取要添加到Intruder的{@link PayloadGenerator}实例
     *
     * @param attackConfiguration 包含当前选定的攻击配置标签页信息的对象
     * @return 实现{@link PayloadGenerator}接口的对象实例
     */
    PayloadGenerator providePayloadGenerator(AttackConfiguration attackConfiguration);
}
```
### PayloadProcessingAction
```java
/*
 * Copyright (c) 2022-2023. PortSwigger Ltd. All rights reserved.
 *
 * 本代码可用于扩展Burp Suite社区版和专业版的功能，
 * 前提是该使用不违反产品许可条款。
 */

package burp.api.montoya.intruder;

/**
 * 载荷处理器可对当前载荷执行的操作指令
 */
public enum PayloadProcessingAction 
{
    /**
     * 跳过当前载荷（不处理）
     */
    SKIP_PAYLOAD,
    
    /**
     * 使用当前载荷（正常处理） 
     */
    USE_PAYLOAD
}
```
### PayloadProcessingResult
```java
/*
 * Copyright (c) 2022-2023. PortSwigger Ltd. All rights reserved.
 *
 * 本代码可用于扩展Burp Suite社区版和专业版的功能，
 * 前提是该使用不违反产品许可条款。
 */

package burp.api.montoya.intruder;

import burp.api.montoya.core.ByteArray;

import static burp.api.montoya.internal.ObjectFactoryLocator.FACTORY;

/**
 * 载荷处理结果接口。
 * <p>
 * 当自定义的{@link PayloadProcessor}在Intruder中注册后，
 * 应通过{@link PayloadProcessor#processPayload}方法返回此接口的实例。
 */
public interface PayloadProcessingResult
{
    /**
     * 获取处理后的载荷值
     * 
     * @return 处理后的载荷字节数组
     */
    ByteArray processedPayload();

    /**
     * 获取载荷处理动作指令
     * <p>
     * Burp调用此方法决定如何处理载荷：
     * - 返回{@link PayloadProcessingAction#USE_PAYLOAD}时使用该载荷
     * - 返回{@link PayloadProcessingAction#SKIP_PAYLOAD}时跳过该载荷
     *
     * @return 载荷处理动作指令
     */
    PayloadProcessingAction action();

    /**
     * 创建使用载荷的处理结果实例
     *
     * @param processedPayload 处理后的载荷值
     * @return 新的载荷处理结果实例
     */
    static PayloadProcessingResult usePayload(ByteArray processedPayload)
    {
        return FACTORY.usePayload(processedPayload);
    }

    /**
     * 创建跳过载荷的处理结果实例
     * 
     * @return 新的载荷处理结果实例
     */
    static PayloadProcessingResult skipPayload()
    {
        return FACTORY.skipPayload();
    }

    /**
     * 创建自定义动作的载荷处理结果实例
     *
     * @param processedPayload 处理后的载荷值
     * @param action 要执行的处理动作
     * @return 新的载荷处理结果实例
     */
    static PayloadProcessingResult payloadProcessingResult(ByteArray processedPayload, PayloadProcessingAction action)
    {
        return FACTORY.payloadProcessingResult(processedPayload, action);
    }
}
```
### PayloadProcessor
```java
/*
 * Copyright (c) 2022-2023. PortSwigger Ltd. All rights reserved.
 *
 * 本代码可用于扩展Burp Suite社区版和专业版的功能，
 * 前提是该使用不违反产品许可条款。
 */

package burp.api.montoya.intruder;

/**
 * 载荷处理器接口。
 * <p>
 * 扩展程序可实现此接口，并通过调用{@link Intruder#registerPayloadProcessor}方法
 * 来注册自定义的Intruder载荷处理器。
 */
public interface PayloadProcessor
{
    /**
     * 获取处理器在UI下拉列表中显示的名称
     * 
     * @return 载荷处理器的显示名称
     */
    String displayName();

    /**
     * 处理Intruder载荷的核心方法。
     * <p>
     * 当需要处理载荷时，Burp会调用此方法。
     *
     * @param payloadData 包含当前待处理载荷信息的对象
     * @return 载荷处理结果，包含处理后的值和操作指令
     */
    PayloadProcessingResult processPayload(PayloadData payloadData);
}

```
## logger
### LoggerCaptureHttpRequestResponse
```java
/*
 * Copyright (c) 2022-2023. PortSwigger Ltd. All rights reserved.
 *
 * 本代码可用于扩展Burp Suite社区版和专业版的功能，
 * 前提是该使用不违反产品许可条款。
 */

package burp.api.montoya.logger;

import burp.api.montoya.core.ToolSource;
import burp.api.montoya.http.HttpService;
import burp.api.montoya.http.handler.TimingData;
import burp.api.montoya.http.message.MimeType;
import burp.api.montoya.http.message.requests.HttpRequest;
import burp.api.montoya.http.message.responses.HttpResponse;

import java.time.ZonedDateTime;
import java.util.regex.Pattern;

/**
 * HTTP请求/响应记录条目接口。
 * <p>
 * 定义可被Logger记录的HTTP请求和响应之间的关联关系。
 */
public interface LoggerCaptureHttpRequestResponse
{
    /**
     * 获取HTTP请求消息
     * @return HTTP请求对象
     */
    HttpRequest request();

    /**
     * 获取HTTP响应消息
     * @return HTTP响应对象（可能为null）
     */
    HttpResponse response();

    /**
     * 获取请求的HTTP服务信息
     * @return 包含HTTP服务详细信息的对象
     */
    HttpService httpService();

    /**
     * 获取Logger收到请求的时间
     * @return 请求接收时间
     */
    ZonedDateTime time();

    /**
     * 获取Burp判定的响应或请求的MIME类型
     * <p>
     * 若无响应，则根据请求URL确定MIME类型
     * @return MIME类型枚举值
     */
    MimeType mimeType();

    /**
     * 检查是否存在响应
     * @return 存在响应返回true
     */
    boolean hasResponse();

    /**
     * 获取请求的计时数据
     * @return 计时数据对象
     */
    TimingData timingData();

    /**
     * 获取响应页面标题
     * @return 页面标题字符串（无标题返回空字符串）
     */
    String pageTitle();

    /**
     * 获取发起请求的工具来源
     * @return 工具来源枚举值
     */
    ToolSource toolSource();

    /**
     * 在请求/响应数据中搜索指定内容
     * @param searchTerm 要搜索的内容
     * @param caseSensitive 是否区分大小写
     * @return 找到返回true
     */
    boolean contains(String searchTerm, boolean caseSensitive);

    /**
     * 使用正则表达式搜索请求/响应数据
     * @param pattern 正则表达式
     * @return 匹配成功返回true
     */
    boolean contains(Pattern pattern);

    /**
     * 检查是否为会话处理请求
     * @return 是会话处理请求返回true
     */
    boolean isSessionHandlingEvent();
}
```
### LoggerHttpRequestResponse
```java
/*
 * Copyright (c) 2022-2023. PortSwigger Ltd. All rights reserved.
 *
 * 本代码可用于扩展Burp Suite社区版和专业版的功能，
 * 前提是该使用不违反产品许可条款。
 */

package burp.api.montoya.logger;

import burp.api.montoya.core.Annotations;
import burp.api.montoya.core.ToolSource;
import burp.api.montoya.http.HttpService;
import burp.api.montoya.http.handler.TimingData;
import burp.api.montoya.http.message.MimeType;
import burp.api.montoya.http.message.requests.HttpRequest;
import burp.api.montoya.http.message.responses.HttpResponse;

import java.time.ZonedDateTime;
import java.util.regex.Pattern;

/**
 * Logger模块的HTTP请求/响应记录接口。
 * <p>
 * 定义Logger中HTTP请求和响应的关联关系及元数据信息。
 */
public interface LoggerHttpRequestResponse
{
    /**
     * 获取HTTP请求对象
     * @return HTTP请求实例
     */
    HttpRequest request();

    /**
     * 获取HTTP响应对象
     * @return HTTP响应实例（可能为null）
     */
    HttpResponse response();

    /**
     * 获取请求的目标服务信息
     * @return 包含主机、端口和协议的服务对象
     */
    HttpService httpService();

    /**
     * 获取请求/响应的注释信息
     * @return 注释对象
     */
    Annotations annotations();

    /**
     * 获取Logger记录该请求的时间
     * @return 带时区的时间对象
     */
    ZonedDateTime time();

    /**
     * 获取Burp自动判定的内容类型
     * <p>
     * 若无响应，则根据请求URL推测MIME类型
     * @return MIME类型枚举值
     */
    MimeType mimeType();

    /**
     * 检查是否存在响应
     * @return 存在响应返回true
     */
    boolean hasResponse();

    /**
     * 获取请求的计时信息
     * @return 包含RTT等计时数据的对象
     */
    TimingData timingData();

    /**
     * 获取HTML响应的页面标题
     * @return 页面标题（无标题返回空字符串）
     */
    String pageTitle();

    /**
     * 获取发起请求的Burp工具来源
     * @return 工具来源枚举值
     */
    ToolSource toolSource();

    /**
     * 全文搜索请求/响应数据
     * @param searchTerm 搜索关键词
     * @param caseSensitive 是否区分大小写
     * @return 匹配成功返回true
     */
    boolean contains(String searchTerm, boolean caseSensitive);

    /**
     * 使用正则表达式搜索请求/响应数据
     * @param pattern 正则表达式对象
     * @return 匹配成功返回true
     */
    boolean contains(Pattern pattern);
}
```
## logging
### Logging
```java
/*
 * Copyright (c) 2022-2023. PortSwigger Ltd. All rights reserved.
 *
 * 本代码可用于扩展Burp Suite社区版和专业版的功能，
 * 前提是该使用不违反产品许可条款。
 */

package burp.api.montoya.logging;

import java.io.PrintStream;

/**
 * 日志记录功能接口。
 * <p>
 * 提供扩展程序的日志记录和事件通知功能。
 */
public interface Logging
{
    /**
     * 获取扩展程序的标准输出流（已弃用）。
     * <p>
     * 扩展程序应通过此流输出信息，用户可在UI中配置输出处理方式。
     *
     * @return 标准输出流对象
     * @deprecated 请使用 {@link burp.api.montoya.logging.Logging#logToOutput} 替代
     */
    @Deprecated
    PrintStream output();

    /**
     * 获取扩展程序的标准错误流（已弃用）。
     * <p>
     * 扩展程序应通过此流输出错误信息，用户可在UI中配置输出处理方式。
     *
     * @return 标准错误流对象
     * @deprecated 请使用 {@link burp.api.montoya.logging.Logging#logToError} 替代
     */
    @Deprecated
    PrintStream error();

    /**
     * 输出日志信息到标准输出流。
     *
     * @param message 要输出的日志信息
     */
    void logToOutput(String message);

    /**
     * 输出对象信息到标准输出流。
     *
     * @param object 要输出的对象
     */
    void logToOutput(Object object);

    /**
     * 输出错误信息到标准错误流。
     *
     * @param message 要输出的错误信息
     */
    void logToError(String message);

    /**
     * 输出错误信息和异常堆栈到标准错误流。
     *
     * @param message 错误描述信息
     * @param cause 导致错误的异常对象
     */
    void logToError(String message, Throwable cause);

    /**
     * 输出异常堆栈到标准错误流。
     *
     * @param cause 导致错误的异常对象
     */
    void logToError(Throwable cause);

    /**
     * 在Burp事件日志中记录调试级别事件。
     *
     * @param message 调试信息
     */
    void raiseDebugEvent(String message);

    /**
     * 在Burp事件日志中记录信息级别事件。
     *
     * @param message 提示信息
     */
    void raiseInfoEvent(String message);

    /**
     * 在Burp事件日志中记录错误级别事件。
     *
     * @param message 错误信息
     */
    void raiseErrorEvent(String message);

    /**
     * 在Burp事件日志中记录严重级别事件。
     *
     * @param message 严重错误信息
     */
    void raiseCriticalEvent(String message);
}
```
## organizer
### Organizer
```java
/*
 * Copyright (c) 2022-2023. PortSwigger Ltd. All rights reserved.
 *
 * 本代码可用于扩展Burp Suite社区版和专业版的功能，
 * 前提是该使用不违反产品许可条款。
 */

package burp.api.montoya.organizer;

import burp.api.montoya.http.message.HttpRequestResponse;
import burp.api.montoya.http.message.requests.HttpRequest;

/**
 * Organizer工具功能接口。
 * <p>
 * 提供对Burp Organizer工具功能的访问能力。
 */
public interface Organizer
{
    /**
     * 发送HTTP请求到Organizer工具。
     * <p>
     * 该请求将在Organizer界面中显示并可用于后续分析。
     *
     * @param request 要发送的完整HTTP请求
     */
    void sendToOrganizer(HttpRequest request);

    /**
     * 发送HTTP请求和响应到Organizer工具。
     * <p>
     * 该请求和响应将在Organizer界面中显示并可用于后续分析。
     *
     * @param requestResponse 包含完整HTTP请求和响应的对象
     */
    void sendToOrganizer(HttpRequestResponse requestResponse);
}
```
## persistence
### PersistedList
```java
/*
 * Copyright (c) 2022-2023. PortSwigger Ltd. All rights reserved.
 *
 * 本代码可用于扩展Burp Suite社区版和专业版的功能，
 * 前提是该使用不违反产品许可条款。
 */

package burp.api.montoya.persistence;

import burp.api.montoya.core.ByteArray;
import burp.api.montoya.http.message.HttpRequestResponse;
import burp.api.montoya.http.message.requests.HttpRequest;
import burp.api.montoya.http.message.responses.HttpResponse;

import java.util.List;

import static burp.api.montoya.internal.ObjectFactoryLocator.FACTORY;

/**
 * 持久化列表接口。
 * <p>
 * 表示项目中持久化存储的列表数据，所有操作都会直接影响底层持久化数据。
 */
public interface PersistedList<T> extends List<T>
{
    /**
     * 创建存储Boolean类型的持久化列表
     * @return 新的持久化列表实例
     */
    static PersistedList<Boolean> persistedBooleanList()
    {
        return FACTORY.persistedBooleanList();
    }

    /**
     * 创建存储Short类型的持久化列表
     * @return 新的持久化列表实例
     */
    static PersistedList<Short> persistedShortList()
    {
        return FACTORY.persistedShortList();
    }

    /**
     * 创建存储Integer类型的持久化列表
     * @return 新的持久化列表实例
     */
    static PersistedList<Integer> persistedIntegerList()
    {
        return FACTORY.persistedIntegerList();
    }

    /**
     * 创建存储Long类型的持久化列表
     * @return 新的持久化列表实例
     */
    static PersistedList<Long> persistedLongList()
    {
        return FACTORY.persistedLongList();
    }

    /**
     * 创建存储String类型的持久化列表
     * @return 新的持久化列表实例
     */
    static PersistedList<String> persistedStringList()
    {
        return FACTORY.persistedStringList();
    }

    /**
     * 创建存储ByteArray类型的持久化列表
     * @return 新的持久化列表实例
     */
    static PersistedList<ByteArray> persistedByteArrayList()
    {
        return FACTORY.persistedByteArrayList();
    }

    /**
     * 创建存储HttpRequest类型的持久化列表
     * @return 新的持久化列表实例
     */
    static PersistedList<HttpRequest> persistedHttpRequestList()
    {
        return FACTORY.persistedHttpRequestList();
    }

    /**
     * 创建存储HttpResponse类型的持久化列表
     * @return 新的持久化列表实例
     */
    static PersistedList<HttpResponse> persistedHttpResponseList()
    {
        return FACTORY.persistedHttpResponseList();
    }

    /**
     * 创建存储HttpRequestResponse类型的持久化列表
     * @return 新的持久化列表实例
     */
    static PersistedList<HttpRequestResponse> persistedHttpRequestResponseList()
    {
        return FACTORY.persistedHttpRequestResponseList();
    }
}
```
### PersistedObject
```java
/*
 * Copyright (c) 2022-2023. PortSwigger Ltd. All rights reserved.
 *
 * 本代码可用于扩展Burp Suite社区版和专业版的功能，
 * 前提是该使用不违反产品许可条款。
 */

package burp.api.montoya.persistence;

import burp.api.montoya.core.ByteArray;
import burp.api.montoya.http.message.HttpRequestResponse;
import burp.api.montoya.http.message.requests.HttpRequest;
import burp.api.montoya.http.message.responses.HttpResponse;

import java.util.Set;

import static burp.api.montoya.internal.ObjectFactoryLocator.FACTORY;

/**
 * 持久化对象接口（仅限专业版）。
 * <p>
 * 支持将HTTP请求、响应、字节数组、基本类型及其列表等数据持久化存储到Burp项目中。
 */
public interface PersistedObject
{
    /**
     * 获取指定键关联的子持久化对象
     * @param key 要查找的键
     * @return 关联的持久化对象，未找到返回null
     */
    PersistedObject getChildObject(String key);

    /**
     * 设置键值关联的子持久化对象
     * @param key 键名
     * @param childObject 要关联的持久化对象
     */
    void setChildObject(String key, PersistedObject childObject);

    /**
     * 删除指定键关联的子持久化对象
     * @param key 要删除的键
     */
    void deleteChildObject(String key);

    /**
     * 获取所有子持久化对象的键集合
     * @return 键集合
     */
    Set<String> childObjectKeys();

    /**
     * 获取字符串值
     * @param key 键名
     * @return 字符串值，未找到返回null
     */
    String getString(String key);

    /**
     * 设置字符串值
     * @param key 键名
     * @param value 字符串值
     */
    void setString(String key, String value);

    /**
     * 删除字符串值
     * @param key 要删除的键
     */
    void deleteString(String key);

    /**
     * 获取所有字符串键集合
     * @return 键集合
     */
    Set<String> stringKeys();

    /**
     * 获取布尔值
     * @param key 键名
     * @return 布尔值，未找到返回null
     */
    Boolean getBoolean(String key);

    /**
     * 设置布尔值
     * @param key 键名
     * @param value 布尔值
     */
    void setBoolean(String key, boolean value);

    /**
     * 删除布尔值
     * @param key 要删除的键
     */
    void deleteBoolean(String key);

    /**
     * 获取所有布尔值键集合
     * @return 键集合
     */
    Set<String> booleanKeys();

    /**
     * 获取字节值
     * @param key 键名
     * @return 字节值，未找到返回null
     */
    Byte getByte(String key);

    /**
     * 设置字节值
     * @param key 键名
     * @param value 字节值
     */
    void setByte(String key, byte value);

    /**
     * 删除字节值
     * @param key 要删除的键
     */
    void deleteByte(String key);

    /**
     * 获取所有字节值键集合
     * @return 键集合
     */
    Set<String> byteKeys();

    /**
     * 获取短整型值
     * @param key 键名
     * @return 短整型值，未找到返回null
     */
    Short getShort(String key);

    /**
     * 设置短整型值
     * @param key 键名
     * @param value 短整型值
     */
    void setShort(String key, short value);

    /**
     * 删除短整型值
     * @param key 要删除的键
     */
    void deleteShort(String key);

    /**
     * 获取所有短整型值键集合
     * @return 键集合
     */
    Set<String> shortKeys();

    /**
     * 获取整型值
     * @param key 键名
     * @return 整型值，未找到返回null
     */
    Integer getInteger(String key);

    /**
     * 设置整型值
     * @param key 键名
     * @param value 整型值
     */
    void setInteger(String key, int value);

    /**
     * 删除整型值
     * @param key 要删除的键
     */
    void deleteInteger(String key);

    /**
     * 获取所有整型值键集合
     * @return 键集合
     */
    Set<String> integerKeys();

    /**
     * 获取长整型值
     * @param key 键名
     * @return 长整型值，未找到返回null
     */
    Long getLong(String key);

    /**
     * 设置长整型值
     * @param key 键名
     * @param value 长整型值
     */
    void setLong(String key, long value);

    /**
     * 删除长整型值
     * @param key 要删除的键
     */
    void deleteLong(String key);

    /**
     * 获取所有长整型值键集合
     * @return 键集合
     */
    Set<String> longKeys();

    /**
     * 获取字节数组值
     * @param key 键名
     * @return 字节数组，未找到返回null
     */
    ByteArray getByteArray(String key);

    /**
     * 设置字节数组值
     * @param key 键名
     * @param value 字节数组
     */
    void setByteArray(String key, ByteArray value);

    /**
     * 删除字节数组值
     * @param key 要删除的键
     */
    void deleteByteArray(String key);

    /**
     * 获取所有字节数组键集合
     * @return 键集合
     */
    Set<String> byteArrayKeys();

    /**
     * 获取HTTP请求
     * @param key 键名
     * @return HTTP请求对象，未找到返回null
     */
    HttpRequest getHttpRequest(String key);

    /**
     * 设置HTTP请求
     * @param key 键名
     * @param value HTTP请求对象
     */
    void setHttpRequest(String key, HttpRequest value);

    /**
     * 删除HTTP请求
     * @param key 要删除的键
     */
    void deleteHttpRequest(String key);

    /**
     * 获取所有HTTP请求键集合
     * @return 键集合
     */
    Set<String> httpRequestKeys();

    /**
     * 获取HTTP请求列表
     * @param key 键名
     * @return 持久化列表，未找到返回null
     */
    PersistedList<HttpRequest> getHttpRequestList(String key);

    /**
     * 设置HTTP请求列表
     * @param key 键名
     * @param value 持久化列表
     */
    void setHttpRequestList(String key, PersistedList<HttpRequest> value);

    /**
     * 删除HTTP请求列表
     * @param key 要删除的键
     */
    void deleteHttpRequestList(String key);

    /**
     * 获取所有HTTP请求列表键集合
     * @return 键集合
     */
    Set<String> httpRequestListKeys();

    /**
     * 获取HTTP响应
     * @param key 键名
     * @return HTTP响应对象，未找到返回null
     */
    HttpResponse getHttpResponse(String key);

    /**
     * 设置HTTP响应
     * @param key 键名
     * @param value HTTP响应对象
     */
    void setHttpResponse(String key, HttpResponse value);

    /**
     * 删除HTTP响应
     * @param key 要删除的键
     */
    void deleteHttpResponse(String key);

    /**
     * 获取所有HTTP响应键集合
     * @return 键集合
     */
    Set<String> httpResponseKeys();

    /**
     * 获取HTTP响应列表
     * @param key 键名
     * @return 持久化列表，未找到返回null
     */
    PersistedList<HttpResponse> getHttpResponseList(String key);

    /**
     * 设置HTTP响应列表
     * @param key 键名
     * @param value 持久化列表
     */
    void setHttpResponseList(String key, PersistedList<HttpResponse> value);

    /**
     * 删除HTTP响应列表
     * @param key 要删除的键
     */
    void deleteHttpResponseList(String key);

    /**
     * 获取所有HTTP响应列表键集合
     * @return 键集合
     */
    Set<String> httpResponseListKeys();

    /**
     * 获取HTTP请求响应对
     * @param key 键名
     * @return HTTP请求响应对象，未找到返回null
     */
    HttpRequestResponse getHttpRequestResponse(String key);

    /**
     * 设置HTTP请求响应对
     * @param key 键名
     * @param value HTTP请求响应对象
     */
    void setHttpRequestResponse(String key, HttpRequestResponse value);

    /**
     * 删除HTTP请求响应对
     * @param key 要删除的键
     */
    void deleteHttpRequestResponse(String key);

    /**
     * 获取所有HTTP请求响应对键集合
     * @return 键集合
     */
    Set<String> httpRequestResponseKeys();

    /**
     * 获取HTTP请求响应对列表
     * @param key 键名
     * @return 持久化列表，未找到返回null
     */
    PersistedList<HttpRequestResponse> getHttpRequestResponseList(String key);

    /**
     * 设置HTTP请求响应对列表
     * @param key 键名
     * @param value 持久化列表
     */
    void setHttpRequestResponseList(String key, PersistedList<HttpRequestResponse> value);

    /**
     * 删除HTTP请求响应对列表
     * @param key 要删除的键
     */
    void deleteHttpRequestResponseList(String key);

    /**
     * 获取所有HTTP请求响应对列表键集合
     * @return 键集合
     */
    Set<String> httpRequestResponseListKeys();

    /**
     * 获取布尔值列表
     * @param key 键名
     * @return 持久化列表，未找到返回null
     */
    PersistedList<Boolean> getBooleanList(String key);

    /**
     * 设置布尔值列表
     * @param key 键名
     * @param value 持久化列表
     */
    void setBooleanList(String key, PersistedList<Boolean> value);

    /**
     * 删除布尔值列表
     * @param key 要删除的键
     */
    void deleteBooleanList(String key);

    /**
     * 获取所有布尔值列表键集合
     * @return 键集合
     */
    Set<String> booleanListKeys();

    /**
     * 获取短整型值列表
     * @param key 键名
     * @return 持久化列表，未找到返回null
     */
    PersistedList<Short> getShortList(String key);

    /**
     * 设置短整型值列表
     * @param key 键名
     * @param value 持久化列表
     */
    void setShortList(String key, PersistedList<Short> value);

    /**
     * 删除短整型值列表
     * @param key 要删除的键
     */
    void deleteShortList(String key);

    /**
     * 获取所有短整型值列表键集合
     * @return 键集合
     */
    Set<String> shortListKeys();

    /**
     * 获取整型值列表
     * @param key 键名
     * @return 持久化列表，未找到返回null
     */
    PersistedList<Integer> getIntegerList(String key);

    /**
     * 设置整型值列表
     * @param key 键名
     * @param value 持久化列表
     */
    void setIntegerList(String key, PersistedList<Integer> value);

    /**
     * 删除整型值列表
     * @param key 要删除的键
     */
    void deleteIntegerList(String key);

    /**
     * 获取所有整型值列表键集合
     * @return 键集合
     */
    Set<String> integerListKeys();

    /**
     * 获取长整型值列表
     * @param key 键名
     * @return 持久化列表，未找到返回null
     */
    PersistedList<Long> getLongList(String key);

    /**
     * 设置长整型值列表
     * @param key 键名
     * @param value 持久化列表
     */
    void setLongList(String key, PersistedList<Long> value);

    /**
     * 删除长整型值列表
     * @param key 要删除的键
     */
    void deleteLongList(String key);

    /**
     * 获取所有长整型值列表键集合
     * @return 键集合
     */
    Set<String> longListKeys();

    /**
     * 获取字符串列表
     * @param key 键名
     * @return 持久化列表，未找到返回null
     */
    PersistedList<String> getStringList(String key);

    /**
     * 设置字符串列表
     * @param key 键名
     * @param value 持久化列表
     */
    void setStringList(String key, PersistedList<String> value);

    /**
     * 删除字符串列表
     * @param key 要删除的键
     */
    void deleteStringList(String key);

    /**
     * 获取所有字符串列表键集合
     * @return 键集合
     */
    Set<String> stringListKeys();

    /**
     * 获取字节数组列表
     * @param key 键名
     * @return 持久化列表，未找到返回null
     */
    PersistedList<ByteArray> getByteArrayList(String key);

    /**
     * 设置字节数组列表
     * @param key 键名
     * @param value 持久化列表
     */
    void setByteArrayList(String key, PersistedList<ByteArray> value);

    /**
     * 删除字节数组列表
     * @param key 要删除的键
     */
    void deleteByteArrayList(String key);

    /**
     * 获取所有字节数组列表键集合
     * @return 键集合
     */
    Set<String> byteArrayListKeys();

    /**
     * 创建新的持久化对象实例
     * @return 新实例
     */
    static PersistedObject persistedObject()
    {
        return FACTORY.persistedObject();
    }
}
```
### Persistence
```java
/*
 * Copyright (c) 2022-2023. PortSwigger Ltd. All rights reserved.
 *
 * 本代码可用于扩展Burp Suite社区版和专业版的功能，
 * 前提是该使用不违反产品许可条款。
 */

package burp.api.montoya.persistence;

/**
 * 持久化功能接口。
 * <p>
 * 提供对Burp Suite持久化功能的访问。
 */
public interface Persistence
{
    /**
     * 获取扩展数据存储功能。
     * <p>
     * 当Burp在没有项目文件的情况下启动时，数据将存储在内存中。
     *
     * @return 实现了{@link PersistedObject}接口的对象，
     *         用于在项目文件或内存中存储数据
     */
    PersistedObject extensionData();

    /**
     * 获取Java偏好设置存储功能。
     * <p>
     * 该存储方式在扩展重载和Burp Suite重启后仍然保持。
     *
     * @return 实现了{@link Preferences}接口的对象，
     *         用于持久化存储数据
     */
    Preferences preferences();
}
```
### Preferences
```java
/*
 * Copyright (c) 2022-2023. PortSwigger Ltd. All rights reserved.
 *
 * 本代码可用于扩展Burp Suite社区版和专业版的功能，
 * 前提是该使用不违反产品许可条款。
 */

package burp.api.montoya.persistence;

import java.util.Set;

/**
 * Java偏好设置存储接口。
 * <p>
 * 支持基本数据类型的持久化存储和访问。
 */
public interface Preferences
{
    /**
     * 获取字符串值
     * @param key 键名
     * @return 字符串值，未找到返回null
     */
    String getString(String key);

    /**
     * 设置字符串值
     * @param key 键名
     * @param value 字符串值
     */
    void setString(String key, String value);

    /**
     * 删除字符串值
     * @param key 要删除的键
     */
    void deleteString(String key);

    /**
     * 获取所有字符串键集合
     * @return 键集合
     */
    Set<String> stringKeys();

    /**
     * 获取布尔值
     * @param key 键名
     * @return 布尔值，未找到返回null
     */
    Boolean getBoolean(String key);

    /**
     * 设置布尔值
     * @param key 键名
     * @param value 布尔值
     */
    void setBoolean(String key, boolean value);

    /**
     * 删除布尔值
     * @param key 要删除的键
     */
    void deleteBoolean(String key);

    /**
     * 获取所有布尔值键集合
     * @return 键集合
     */
    Set<String> booleanKeys();

    /**
     * 获取字节值
     * @param key 键名
     * @return 字节值，未找到返回null
     */
    Byte getByte(String key);

    /**
     * 设置字节值
     * @param key 键名
     * @param value 字节值
     */
    void setByte(String key, byte value);

    /**
     * 删除字节值
     * @param key 要删除的键
     */
    void deleteByte(String key);

    /**
     * 获取所有字节值键集合
     * @return 键集合
     */
    Set<String> byteKeys();

    /**
     * 获取短整型值
     * @param key 键名
     * @return 短整型值，未找到返回null
     */
    Short getShort(String key);

    /**
     * 设置短整型值
     * @param key 键名
     * @param value 短整型值
     */
    void setShort(String key, short value);

    /**
     * 删除短整型值
     * @param key 要删除的键
     */
    void deleteShort(String key);

    /**
     * 获取所有短整型值键集合
     * @return 键集合
     */
    Set<String> shortKeys();

    /**
     * 获取整型值
     * @param key 键名
     * @return 整型值，未找到返回null
     */
    Integer getInteger(String key);

    /**
     * 设置整型值
     * @param key 键名
     * @param value 整型值
     */
    void setInteger(String key, int value);

    /**
     * 删除整型值
     * @param key 要删除的键
     */
    void deleteInteger(String key);

    /**
     * 获取所有整型值键集合
     * @return 键集合
     */
    Set<String> integerKeys();

    /**
     * 获取长整型值
     * @param key 键名
     * @return 长整型值，未找到返回null
     */
    Long getLong(String key);

    /**
     * 设置长整型值
     * @param key 键名
     * @param value 长整型值
     */
    void setLong(String key, long value);

    /**
     * 删除长整型值
     * @param key 要删除的键
     */
    void deleteLong(String key);

    /**
     * 获取所有长整型值键集合
     * @return 键集合
     */
    Set<String> longKeys();
}
```
## project
### Project
```java
package burp.api.montoya.project;

/**
 * 项目功能接口。
 * <p>
 * 提供对当前Burp项目相关功能的访问。
 */
public interface Project
{
    /**
     * 获取当前项目名称。
     * 
     * @return 项目名称字符串
     */
    String name();

    /**
     * 获取当前项目的唯一标识符。
     * 
     * @return 项目唯一ID字符串
     */
    String id();
}
```
## proxy
### MessageReceivedAction
```java
/*
 * Copyright (c) 2022-2023. PortSwigger Ltd. All rights reserved.
 *
 * 本代码可用于扩展Burp Suite社区版和专业版的功能，
 * 前提是该使用不违反产品许可条款。
 */

package burp.api.montoya.proxy;

/**
 * 代理拦截动作枚举。
 * <p>
 * 表示Proxy拦截HTTP和WebSocket消息时的初始处理动作。
 */
public enum MessageReceivedAction
{
    /**
     * 遵循当前拦截规则处理消息。
     * <p>
     * Burp Proxy将根据配置的拦截规则决定如何处理该消息。
     */
    CONTINUE,

    /**
     * 拦截消息并交由用户手动处理。
     * <p>
     * 消息将被暂停在拦截队列中，等待用户查看或修改。
     */
    INTERCEPT,

    /**
     * 不拦截直接转发消息。
     * <p>
     * 消息将绕过拦截队列直接转发到目标服务器/客户端。
     */
    DO_NOT_INTERCEPT,

    /**
     * 丢弃消息。
     * <p>
     * 消息将被直接丢弃，不会转发到目标。
     */
    DROP
}
```
### MessageToBeSentAction

```java
/*
 * Copyright (c) 2022-2023. PortSwigger Ltd. All rights reserved.
 *
 * 本代码可用于扩展Burp Suite社区版和专业版的功能，
 * 前提是该使用不违反产品许可条款。
 */

package burp.api.montoya.proxy;

/**
 * 代理发送动作枚举。
 * <p>
 * 表示Proxy处理完拦截的HTTP和WebSocket消息后要执行的最终动作。
 */
public enum MessageToBeSentAction
{
    /**
     * 继续转发消息。
     * <p>
     * 消息将被正常转发到目标服务器/客户端。
     */
    CONTINUE,

    /**
     * 丢弃消息。
     * <p>
     * 消息将被直接丢弃，不会转发到目标。
     */
    DROP
}
```
### Proxy
```java
/*
 * Copyright (c) 2022-2023. PortSwigger Ltd. All rights reserved.
 *
 * 本代码可用于扩展Burp Suite社区版和专业版的功能，
 * 前提是该使用不违反产品许可条款。
 */

package burp.api.montoya.proxy;

import burp.api.montoya.core.Registration;
import burp.api.montoya.proxy.http.ProxyRequestHandler;
import burp.api.montoya.proxy.http.ProxyResponseHandler;
import burp.api.montoya.proxy.websocket.ProxyWebSocketCreationHandler;

import java.util.List;

/**
 * 代理工具功能接口。
 * <p>
 * 提供对Burp Proxy工具功能的访问和控制。
 */
public interface Proxy
{
    /**
     * 启用代理主拦截功能。
     * <p>
     * 启用后，Proxy将根据配置拦截HTTP/WebSocket消息。
     */
    void enableIntercept();

    /**
     * 禁用代理主拦截功能。
     * <p>
     * 禁用后，Proxy将不再拦截任何消息。
     */
    void disableIntercept();

    /**
     * 检查代理主拦截功能是否启用。
     * @return 如果拦截功能启用返回true，否则返回false
     */
    boolean isInterceptEnabled();

    /**
     * 获取Proxy历史记录中的所有HTTP请求/响应。
     * @return 包含所有历史记录的ProxyHttpRequestResponse列表
     */
    List<ProxyHttpRequestResponse> history();

    /**
     * 根据过滤条件获取Proxy历史记录中的HTTP请求/响应。
     * @param filter 用于过滤历史记录的ProxyHistoryFilter实例
     * @return 符合过滤条件的ProxyHttpRequestResponse列表
     */
    List<ProxyHttpRequestResponse> history(ProxyHistoryFilter filter);

    /**
     * 获取Proxy历史记录中的所有WebSocket消息。
     * @return 包含所有历史记录的ProxyWebSocketMessage列表
     */
    List<ProxyWebSocketMessage> webSocketHistory();

    /**
     * 根据过滤条件获取Proxy历史记录中的WebSocket消息。
     * @param filter 用于过滤历史记录的ProxyWebSocketHistoryFilter实例
     * @return 符合过滤条件的ProxyWebSocketMessage列表
     */
    List<ProxyWebSocketMessage> webSocketHistory(ProxyWebSocketHistoryFilter filter);

    /**
     * 注册请求处理程序。
     * <p>
     * 扩展可以通过此处理程序对Proxy处理的请求进行自定义分析或修改，并控制UI中的消息拦截。
     *
     * @param handler 实现ProxyRequestHandler接口的扩展对象
     * @return 处理程序的注册对象
     */
    Registration registerRequestHandler(ProxyRequestHandler handler);

    /**
     * 注册响应处理程序。
     * <p>
     * 扩展可以通过此处理程序对Proxy处理的响应进行自定义分析或修改，并控制UI中的消息拦截。
     *
     * @param handler 实现ProxyResponseHandler接口的扩展对象
     * @return 处理程序的注册对象
     */
    Registration registerResponseHandler(ProxyResponseHandler handler);

    /**
     * 注册WebSocket创建处理程序。
     * <p>
     * 当Proxy创建WebSocket连接时，将调用此处理程序。
     *
     * @param handler 实现ProxyWebSocketCreationHandler接口的扩展对象
     * @return 处理程序的注册对象
     */
    Registration registerWebSocketCreationHandler(ProxyWebSocketCreationHandler handler);
}
```
### ProxyHistoryFilter
```java
/*
 * 版权所有 (c) 2022-2023。PortSwigger Ltd. 保留所有权利。
 *
 * 此代码可用于扩展Burp Suite社区版和Burp Suite专业版的功能，
 * 前提是该使用不违反这些产品的许可条款。
 */

package burp.api.montoya.proxy;

/**
 * 扩展可以实现此接口，然后调用
 * {@link Proxy#history(ProxyHistoryFilter)} 来获取代理历史中经过筛选的项目列表。
 */
public interface ProxyHistoryFilter
{
    /**
     * 此方法会对代理历史中的每个项目调用，以确定
     * 是否应将其包含在筛选后的项目列表中。
     *
     * @param requestResponse 一个 {@link ProxyHttpRequestResponse} 对象，
     *                        扩展可以使用该对象来确定是否应将项目包含在
     *                        筛选后的项目列表中。
     *
     * @return 如果该项目应包含在筛选后的项目列表中，则返回 {@code true}。
     */
    boolean matches(ProxyHttpRequestResponse requestResponse);
}
```
### ProxyHttpRequestResponse
```java
/*
 * 版权所有 (c) 2022-2023。PortSwigger Ltd. 保留所有权利。
 *
 * 此代码可用于扩展Burp Suite社区版和Burp Suite专业版的功能，
 * 前提是该使用不违反这些产品的许可条款。
 */

package burp.api.montoya.proxy;

import burp.api.montoya.core.Annotations;
import burp.api.montoya.http.HttpService;
import burp.api.montoya.http.handler.TimingData;
import burp.api.montoya.http.message.MimeType;
import burp.api.montoya.http.message.requests.HttpRequest;
import burp.api.montoya.http.message.requests.MalformedRequestException;
import burp.api.montoya.http.message.responses.HttpResponse;

import java.time.ZonedDateTime;
import java.util.regex.Pattern;

/**
 * 被Burp Proxy拦截的HTTP请求和响应。
 */
public interface ProxyHttpRequestResponse
{
    /**
     * 获取由Burp Proxy发送的HTTP请求。
     *
     * @return 由Burp Proxy发送的 {@link HttpRequest}。
     * @see ProxyHttpRequestResponse#finalRequest()
     */
    HttpRequest request();

    /**
     * 获取由Burp Proxy发送的最终HTTP请求（可能经过修改）。
     *
     * @return 由Burp Proxy发送的 {@link HttpRequest}。
     */
    HttpRequest finalRequest();

    /**
     * 获取由Burp Proxy接收的HTTP响应。
     *
     * @return 由Burp Proxy接收的 {@link HttpResponse}。
     * @see ProxyHttpRequestResponse#originalResponse()
     */
    HttpResponse response();

    /**
     * 获取由Burp Proxy接收的原始HTTP响应（未经修改）。
     *
     * @return 由Burp Proxy接收的 {@link HttpResponse}。
     */
    HttpResponse originalResponse();

    /**
     * 获取请求/响应对的注释信息。
     *
     * @return 请求/响应对的 {@link Annotations}。
     */
    Annotations annotations();

    /**
     * 获取请求的HTTP服务信息。
     *
     * @return 包含HTTP服务详情的 {@link HttpService} 对象。
     */
    HttpService httpService();

    /**
     * 获取最终请求的URL。
     * 如果请求格式错误，则抛出 {@link MalformedRequestException}。
     *
     * @return 请求中的URL。
     * @throws MalformedRequestException 如果请求格式错误。
     * @deprecated 将在未来版本中移除，建议使用 {@link #finalRequest()} 方法。
     */
    @Deprecated(forRemoval = true)
    String url();

    /**
     * 获取最终请求的HTTP方法。
     * 如果请求格式错误，则抛出 {@link MalformedRequestException}。
     *
     * @return 请求中使用的HTTP方法。
     * @throws MalformedRequestException 如果请求格式错误。
     * @deprecated 将在未来版本中移除，建议使用 {@link #finalRequest()} 方法。
     */
    @Deprecated(forRemoval = true)
    String method();

    /**
     * 获取最终请求的路径和文件名。
     * 如果请求格式错误，则抛出 {@link MalformedRequestException}。
     *
     * @return 请求中的路径和文件名。
     * @throws MalformedRequestException 如果请求格式错误。
     * @deprecated 将在未来版本中移除，建议使用 {@link #finalRequest()} 路径。
     */
    @Deprecated(forRemoval = true)
    String path();

    /**
     * @return 服务的主机名或IP地址。
     * @deprecated 将在未来版本中移除，建议使用 {@link #finalRequest()} 的httpService。
     */
    @Deprecated(forRemoval = true)
    String host();

    /**
     * @return 服务的端口号。
     * @deprecated 将在未来版本中移除，建议使用 {@link #finalRequest()} 的httpService。
     */
    @Deprecated(forRemoval = true)
    int port();

    /**
     * @return 如果连接使用安全协议则返回true，否则返回false。
     * @deprecated 将在未来版本中移除，建议使用 {@link #finalRequest()} 的httpService。
     */
    @Deprecated(forRemoval = true)
    boolean secure();

    /**
     * @return 服务的字符串表示形式。
     * @deprecated 将在未来版本中移除，建议使用 {@link #finalRequest()} 的httpService。
     */
    @Deprecated(forRemoval = true)
    String httpServiceString();

    /**
     * 从HTTP 1.x消息的请求行中解析的HTTP版本文本。
     * HTTP 2消息将返回"HTTP/2"。
     *
     * @return 版本字符串。
     * @deprecated 将在未来版本中移除，建议使用 {@link #finalRequest()} 的httpVersion。
     */
    @Deprecated(forRemoval = true)
    String requestHttpVersion();

    /**
     * 获取最终请求的正文内容。
     *
     * @return 消息正文作为 {@code String}。
     * @deprecated 将在未来版本中移除，建议使用 {@link #finalRequest()} 的body。
     */
    @Deprecated(forRemoval = true)
    String requestBody();

    /**
     * @return 如果请求或响应被编辑过则返回true。
     */
    boolean edited();

    /**
     * 获取Burp Proxy接收请求的日期和时间。
     *
     * @return Burp Proxy接收请求的时间。
     */
    ZonedDateTime time();

    /**
     * 获取用于请求/响应的代理监听端口。
     *
     * @return 代理监听器使用的端口号。
     */
    int listenerPort();

    /**
     * 获取Burp Suite确定的响应或请求的MIME类型。
     * 如果没有响应，则从请求URL确定MIME类型。
     *
     * @return MIME类型。
     */
    MimeType mimeType();

    /**
     * @return 如果存在响应则返回true。
     */
    boolean hasResponse();

    /**
     * 在HTTP请求和响应的数据中搜索指定的搜索词。
     *
     * @param searchTerm    要搜索的值。
     * @param caseSensitive 指定搜索是否区分大小写。
     *
     * @return 如果找到搜索词则返回true。
     */
    boolean contains(String searchTerm, boolean caseSensitive);

    /**
     * 在HTTP请求和响应的数据中搜索指定的正则表达式。
     *
     * @param pattern 要搜索的正则表达式。
     *
     * @return 如果匹配到模式则返回true。
     */
    boolean contains(Pattern pattern);

    /**
     * 获取与此请求和响应关联的计时数据。
     *
     * @return 计时数据。
     */
    TimingData timingData();
}
```
### ProxyWebSocketHistoryFilter
```java
/*
 * 版权所有 (c) 2022-2023。PortSwigger Ltd. 保留所有权利。
 *
 * 此代码可用于扩展Burp Suite社区版和Burp Suite专业版的功能，
 * 前提是该使用不违反这些产品的许可条款。
 */

package burp.api.montoya.proxy;

/**
 * 扩展可以实现此接口，然后调用
 * {@link Proxy#webSocketHistory(ProxyWebSocketHistoryFilter)} 来获取代理WebSocket历史中
 * 经过筛选的项目列表。
 */
public interface ProxyWebSocketHistoryFilter
{
    /**
     * 此方法会对代理WebSocket历史中的每个项目调用，以确定
     * 是否应将其包含在筛选后的项目列表中。
     *
     * @param message 一个 {@link ProxyWebSocketMessage} 对象，
     *                扩展可以使用该对象来确定是否应将项目包含在
     *                筛选后的项目列表中。
     *
     * @return 如果该项目应包含在筛选后的项目列表中，则返回 {@code true}。
     */
    boolean matches(ProxyWebSocketMessage message);
}
```
### ProxyWebSocketMessage
```java
/*
 * 版权所有 (c) 2022-2023。PortSwigger Ltd. 保留所有权利。
 *
 * 此代码可用于扩展Burp Suite社区版和Burp Suite专业版的功能，
 * 前提是该使用不违反这些产品的许可条款。
 */

package burp.api.montoya.proxy;

import burp.api.montoya.core.Annotations;
import burp.api.montoya.core.ByteArray;
import burp.api.montoya.http.message.requests.HttpRequest;
import burp.api.montoya.ui.contextmenu.WebSocketMessage;
import burp.api.montoya.websocket.Direction;

import java.time.ZonedDateTime;
import java.util.regex.Pattern;

/**
 * 被Burp Proxy拦截的WebSocket消息。
 */
public interface ProxyWebSocketMessage extends WebSocketMessage
{
    /**
     * 获取消息的注释信息。
     *
     * @return 消息的 {@link Annotations}。
     */
    @Override
    Annotations annotations();

    /**
     * @return 消息的传输方向。
     */
    @Override
    Direction direction();

    /**
     * @return WebSocket消息的有效载荷。
     */
    @Override
    ByteArray payload();

    /**
     * @return 用于创建WebSocket连接的 {@link HttpRequest}。
     */
    @Override
    HttpRequest upgradeRequest();

    /**
     * @return 此消息所属的WebSocket连接ID。
     */
    int webSocketId();

    /**
     * @return 表示消息发送时间的 {@link ZonedDateTime} 实例。
     */
    ZonedDateTime time();

    /**
     * @return 经过工具和扩展修改后的有效载荷。如果消息未被编辑则返回 {@code null}。
     */
    ByteArray editedPayload();

    /**
     * 获取用于WebSocket消息的代理监听端口。
     *
     * @return 代理监听器使用的端口号。
     */
    int listenerPort();

    /**
     * 在WebSocket消息数据中搜索指定的搜索词。
     *
     * @param searchTerm    要搜索的值。
     * @param caseSensitive 指定搜索是否区分大小写。
     *
     * @return 如果找到搜索词则返回true。
     */
    boolean contains(String searchTerm, boolean caseSensitive);

    /**
     * 在WebSocket消息数据中搜索指定的正则表达式。
     *
     * @param pattern 要搜索的正则表达式。
     *
     * @return 如果匹配到模式则返回true。
     */
    boolean contains(Pattern pattern);
}
```
### http
#### InterceptedHttpMessage
```java
/*
 * 版权所有 (c) 2022-2023。PortSwigger Ltd. 保留所有权利。
 *
 * 此代码可用于扩展Burp Suite社区版和Burp Suite专业版的功能，
 * 前提是该使用不违反这些产品的许可条款。
 */

package burp.api.montoya.proxy.http;

import java.net.InetAddress;

/**
 * 被Burp Proxy拦截的HTTP消息。
 */
public interface InterceptedHttpMessage
{
    /**
     * 获取此请求/响应对的唯一标识符。
     *
     * @return 唯一标识单个请求/响应对的ID。
     * 扩展可以使用此ID来关联请求和响应的详细信息，
     * 并据此对响应消息进行相应处理。
     */
    int messageId();

    /**
     * 获取处理此拦截消息的Burp Proxy监听器名称。
     *
     * @return 处理拦截消息的Burp Proxy监听器名称。
     * 格式与Proxy监听器UI中显示的相同，例如"127.0.0.1:8080"。
     */
    String listenerInterface();

    /**
     * 获取拦截消息的源IP地址。
     *
     * @return 拦截消息的源IP地址。
     */
    InetAddress sourceIpAddress();

    /**
     * 获取拦截消息的目标IP地址。
     *
     * @return 拦截消息的目标IP地址。
     */
    InetAddress destinationIpAddress();
}
```
#### InterceptedRequest
```java
/*
 * 版权所有 (c) 2022-2023。PortSwigger Ltd. 保留所有权利。
 *
 * 此代码可用于扩展Burp Suite社区版和Burp Suite专业版的功能，
 * 前提是该使用不违反这些产品的许可条款。
 */

package burp.api.montoya.proxy.http;

import burp.api.montoya.core.Annotations;
import burp.api.montoya.core.ByteArray;
import burp.api.montoya.core.Marker;
import burp.api.montoya.http.HttpService;
import burp.api.montoya.http.message.ContentType;
import burp.api.montoya.http.message.HttpHeader;
import burp.api.montoya.http.message.params.HttpParameter;
import burp.api.montoya.http.message.params.HttpParameterType;
import burp.api.montoya.http.message.params.ParsedHttpParameter;
import burp.api.montoya.http.message.requests.HttpRequest;
import burp.api.montoya.http.message.requests.HttpTransformation;
import burp.api.montoya.http.message.requests.MalformedRequestException;

import java.net.InetAddress;
import java.util.List;
import java.util.regex.Pattern;

/**
 * 被Burp Proxy拦截的HTTP请求。
 */
public interface InterceptedRequest extends InterceptedHttpMessage, HttpRequest
{
    /**
     * @return 请求/响应的注释信息。
     */
    Annotations annotations();

    /**
     * @return 如果请求在扫描范围内则返回true。
     */
    @Override
    boolean isInScope();

    /**
     * 获取请求的HTTP服务信息。
     *
     * @return 包含HTTP服务详情的 {@link HttpService} 对象。
     */
    @Override
    HttpService httpService();

    /**
     * 获取请求的URL。
     * 如果请求格式错误，则抛出 {@link MalformedRequestException}。
     *
     * @return 请求中的URL。
     * @throws MalformedRequestException 如果请求格式错误。
     */
    @Override
    String url();

    /**
     * 获取请求的HTTP方法。
     * 如果请求格式错误，则抛出 {@link MalformedRequestException}。
     *
     * @return 请求中使用的HTTP方法。
     * @throws MalformedRequestException 如果请求格式错误。
     */
    @Override
    String method();

    /**
     * 获取包含查询参数的请求路径。
     * 如果请求格式错误，则抛出 {@link MalformedRequestException}。
     *
     * @return 包含查询参数的路径。
     * @throws MalformedRequestException 如果请求格式错误。
     */
    @Override
    String path();

    /**
     * 获取不包含查询参数的请求路径。
     * 如果请求格式错误，则抛出 {@link MalformedRequestException}。
     *
     * @return 不包含查询参数的路径。
     * @throws MalformedRequestException 如果请求格式错误。
     */
    @Override
    String pathWithoutQuery();

    /**
     * 从HTTP 1.x消息的请求行中解析的HTTP版本。
     * HTTP 2消息将返回"HTTP/2"。
     *
     * @return HTTP版本字符串。
     */
    @Override
    String httpVersion();

    /**
     * 获取消息中的HTTP头信息。
     *
     * @return HTTP头列表。
     */
    @Override
    List<HttpHeader> headers();

    /**
     * @param header 要检查是否存在的头信息。
     *
     * @return 如果请求中包含该头信息则返回true。
     */
    @Override
    boolean hasHeader(HttpHeader header);

    /**
     * @param name 要查询的头名称。
     *
     * @return 如果请求中包含该名称的头信息则返回true。
     */
    @Override
    boolean hasHeader(String name);

    /**
     * @param name  要检查的头名称。
     * @param value 要检查的头值。
     *
     * @return 如果请求中包含匹配名称和值的头信息则返回true。
     */
    @Override
    boolean hasHeader(String name, String value);

    /**
     * @param name 要获取的头名称。
     *
     * @return 匹配名称的 {@link HttpHeader} 实例，未找到则返回 {@code null}。
     */
    @Override
    HttpHeader header(String name);

    /**
     * @param name 要获取的头名称。
     *
     * @return 匹配名称的头值字符串，未找到则返回 {@code null}。
     */
    @Override
    String headerValue(String name);

    /**
     * @return 如果请求包含参数则返回true。
     */
    @Override
    boolean hasParameters();

    /**
     * @return 如果请求包含指定 {@link HttpParameterType} 类型的参数则返回true。
     */
    @Override
    boolean hasParameters(HttpParameterType type);

    /**
     * @param name 要查找的参数名称。
     * @param type 要查找的参数类型。
     *
     * @return 匹配类型和名称的 {@link ParsedHttpParameter} 实例，未找到则返回 {@code null}。
     */
    @Override
    ParsedHttpParameter parameter(String name, HttpParameterType type);

    /**
     * @param name 要获取值的参数名称。
     * @param type 要获取值的参数类型。
     *
     * @return 匹配名称和类型的参数值，未找到则返回 {@code null}。
     */
    @Override
    String parameterValue(String name, HttpParameterType type);

    /**
     * @param name 要查找的参数名称。
     * @param type 要查找的参数类型。
     *
     * @return 如果存在匹配名称和类型的参数则返回true。
     */
    @Override
    boolean hasParameter(String name, HttpParameterType type);

    /**
     * @param parameter 要匹配的 {@link HttpParameter} 实例。
     *
     * @return 如果存在匹配 {@link HttpParameter} 数据的参数则返回true。
     */
    @Override
    boolean hasParameter(HttpParameter parameter);

    /**
     * @return 请求检测到的内容类型。
     */
    @Override
    ContentType contentType();

    /**
     * @return 请求中包含的所有参数。
     */
    @Override
    List<ParsedHttpParameter> parameters();

    /**
     * @param type 要返回的参数类型。
     *
     * @return 只包含指定类型的 {@link ParsedHttpParameter} 列表。
     */
    @Override
    List<ParsedHttpParameter> parameters(HttpParameterType type);

    /**
     * 获取消息体字节数组。
     *
     * @return 消息体字节数组。
     */
    @Override
    ByteArray body();

    /**
     * 获取消息体字符串。
     *
     * @return 消息体字符串。
     */
    @Override
    String bodyToString();

    /**
     * 获取消息体中消息正文的起始偏移量。
     *
     * @return 消息正文偏移量。
     */
    @Override
    int bodyOffset();

    /**
     * 获取消息标记。
     *
     * @return 标记列表。
     */
    @Override
    List<Marker> markers();

    /**
     * 在HTTP消息数据中搜索指定的搜索词。
     *
     * @param searchTerm    要搜索的值。
     * @param caseSensitive 指定搜索是否区分大小写。
     *
     * @return 如果找到搜索词则返回true。
     */
    @Override
    boolean contains(String searchTerm, boolean caseSensitive);

    /**
     * 在HTTP消息数据中搜索指定的正则表达式。
     *
     * @param pattern 要搜索的正则表达式。
     *
     * @return 如果匹配到模式则返回true。
     */
    @Override
    boolean contains(Pattern pattern);

    /**
     * 获取消息字节数组。
     *
     * @return 消息字节数组。
     */
    @Override
    ByteArray toByteArray();

    /**
     * 获取消息字符串。
     *
     * @return 消息字符串。
     */
    @Override
    String toString();

    /**
     * 将 {@code HttpRequest} 复制到临时文件中。<br>
     * 此方法用于将 {@code HttpRequest} 对象保存到临时文件，
     * 使其不再保留在内存中。扩展可以使用此方法将
     * {@code HttpRequest} 对象转换为适合长期使用的形式。
     *
     * @return 存储在临时文件中的新 {@code HttpRequest} 实例。
     */
    HttpRequest copyToTempFile();

    /**
     * 使用新的服务创建 {@code HttpRequest} 副本。
     *
     * @param service 要添加的 {@link HttpService} 引用。
     *
     * @return 新的 {@code HttpRequest} 实例。
     */
    @Override
    HttpRequest withService(HttpService service);

    /**
     * 使用新路径创建 {@code HttpRequest} 副本。
     *
     * @param path 要使用的路径。
     *
     * @return 更新路径后的新 {@code HttpRequest} 实例。
     */
    @Override
    HttpRequest withPath(String path);

    /**
     * 使用新方法创建 {@code HttpRequest} 副本。
     *
     * @param method 要使用的方法。
     *
     * @return 更新方法后的新 {@code HttpRequest} 实例。
     */
    @Override
    HttpRequest withMethod(String method);

    /**
     * 创建添加或更新头信息的 {@code HttpRequest} 副本。<br>
     * 如果头信息已存在则更新，不存在则添加。
     *
     * @param header 要添加或更新的HTTP头。
     *
     * @return 添加或更新头信息后的新 {@code HttpRequest}。
     */
    @Override
    HttpRequest withHeader(HttpHeader header);

    /**
     * 创建添加或更新头信息的 {@code HttpRequest} 副本。<br>
     * 如果头信息已存在则更新，不存在则添加。
     *
     * @param name  头名称。
     * @param value 头值。
     *
     * @return 添加或更新头信息后的新 {@code HttpRequest}。
     */
    @Override
    HttpRequest withHeader(String name, String value);

    /**
     * 创建添加或更新HTTP参数的 {@code HttpRequest} 副本。<br>
     * 如果参数已存在则更新，不存在则添加。
     *
     * @param parameters 要添加或更新的HTTP参数。
     *
     * @return 添加或更新参数后的新 {@code HttpRequest}。
     */
    @Override
    HttpRequest withParameter(HttpParameter parameters);

    /**
     * 创建添加HTTP参数的 {@code HttpRequest} 副本。
     *
     * @param parameters 要添加的HTTP参数。
     *
     * @return 新的 {@code HttpRequest} 实例。
     */
    @Override
    HttpRequest withAddedParameters(List<? extends HttpParameter> parameters);

    /**
     * 创建添加HTTP参数的 {@code HttpRequest} 副本。
     *
     * @param parameters 要添加的HTTP参数。
     *
     * @return 新的 {@code HttpRequest} 实例。
     */
    @Override
    HttpRequest withAddedParameters(HttpParameter... parameters);

    /**
     * 创建移除HTTP参数的 {@code HttpRequest} 副本。
     *
     * @param parameters 要移除的HTTP参数。
     *
     * @return 新的 {@code HttpRequest} 实例。
     */
    @Override
    HttpRequest withRemovedParameters(List<? extends HttpParameter> parameters);

    /**
     * 创建移除HTTP参数的 {@code HttpRequest} 副本。
     *
     * @param parameters 要移除的HTTP参数。
     *
     * @return 新的 {@code HttpRequest} 实例。
     */
    @Override
    HttpRequest withRemovedParameters(HttpParameter... parameters);

    /**
     * 创建更新HTTP参数的 {@code HttpRequest} 副本。<br>
     *
     * @param parameters 要更新的HTTP参数。
     *
     * @return 新的 {@code HttpRequest} 实例。
     */
    @Override
    HttpRequest withUpdatedParameters(List<? extends HttpParameter> parameters);

    /**
     * 创建更新HTTP参数的 {@code HttpRequest} 副本。<br>
     *
     * @param parameters 要更新的HTTP参数。
     *
     * @return 新的 {@code HttpRequest} 实例。
     */
    @Override
    HttpRequest withUpdatedParameters(HttpParameter... parameters);

    /**
     * 创建应用转换后的 {@code HttpRequest} 副本。
     *
     * @param transformation 要应用的转换。
     *
     * @return 新的 {@code HttpRequest} 实例。
     */
    @Override
    HttpRequest withTransformationApplied(HttpTransformation transformation);

    /**
     * 创建更新正文后的 {@code HttpRequest} 副本。<br>
     * 同时更新Content-Length头。
     *
     * @param body 请求的新正文。
     *
     * @return 新的 {@code HttpRequest} 实例。
     */
    @Override
    HttpRequest withBody(String body);

    /**
     * 创建更新正文后的 {@code HttpRequest} 副本。<br>
     * 同时更新Content-Length头。
     *
     * @param body 请求的新正文字节数组。
     *
     * @return 新的 {@code HttpRequest} 实例。
     */
    @Override
    HttpRequest withBody(ByteArray body);

    /**
     * 创建添加头信息后的 {@code HttpRequest} 副本。
     *
     * @param name  头名称。
     * @param value 头值。
     *
     * @return 添加头信息后的HTTP请求。
     */
    @Override
    HttpRequest withAddedHeader(String name, String value);

    /**
     * 创建添加头信息后的 {@code HttpRequest} 副本。
     *
     * @param header 要添加的 {@link HttpHeader}。
     *
     * @return 添加头信息后的HTTP请求。
     */
    @Override
    HttpRequest withAddedHeader(HttpHeader header);

    /**
     * 创建更新头信息后的 {@code HttpRequest} 副本。
     *
     * @param name  要更新的头名称。
     * @param value 指定HTTP头的新值。
     *
     * @return 包含更新头信息后的请求。
     */
    @Override
    HttpRequest withUpdatedHeader(String name, String value);

    /**
     * 创建更新头信息后的 {@code HttpRequest} 副本。
     *
     * @param header 包含新值的 {@link HttpHeader}。
     *
     * @return 包含更新头信息后的请求。
     */
    @Override
    HttpRequest withUpdatedHeader(HttpHeader header);

    /**
     * 从当前请求中移除现有的HTTP头。
     *
     * @param name 要从请求中移除的HTTP头名称。
     *
     * @return 包含移除头信息后的请求。
     */
    @Override
    HttpRequest withRemovedHeader(String name);

    /**
     * 从当前请求中移除现有的HTTP头。
     *
     * @param header 要从请求中移除的 {@link HttpHeader}。
     *
     * @return 包含移除头信息后的请求。
     */
    @Override
    HttpRequest withRemovedHeader(HttpHeader header);

    /**
     * 创建添加标记后的 {@code HttpRequest} 副本。
     *
     * @param markers 要添加的请求标记。
     *
     * @return 新的 {@link HttpRequest} 实例。
     */
    @Override
    HttpRequest withMarkers(List<Marker> markers);

    /**
     * 创建添加标记后的 {@code HttpRequest} 副本。
     *
     * @param markers 要添加的请求标记。
     *
     * @return 新的 {@link HttpRequest} 实例。
     */
    @Override
    HttpRequest withMarkers(Marker... markers);

    /**
     * 创建添加默认头信息后的 {@code HttpRequest} 副本。
     *
     * @return 添加默认头信息后的新 {@link HttpRequest}。
     */
    @Override
    HttpRequest withDefaultHeaders();

    /**
     * 获取此请求/响应对的唯一标识符。
     *
     * @return 唯一标识单个请求/响应对的ID。
     * 扩展可以使用此ID来关联请求和响应的详细信息，
     * 并据此对响应消息进行相应处理。
     */
    @Override
    int messageId();

    /**
     * 获取处理此拦截消息的Burp Proxy监听器名称。
     *
     * @return 处理拦截消息的Burp Proxy监听器名称。
     * 格式与Proxy监听器UI中显示的相同，例如"127.0.0.1:8080"。
     */
    @Override
    String listenerInterface();

    /**
     * 获取拦截消息的源IP地址。
     *
     * @return 拦截消息的源IP地址。
     */
    @Override
    InetAddress sourceIpAddress();

    /**
     * 获取拦截消息的目标IP地址。
     *
     * @return 拦截消息的目标IP地址。
     */
    @Override
    InetAddress destinationIpAddress();
}
```
#### InterceptedResponse
```java
/*
 * 版权所有 (c) 2022-2023。PortSwigger Ltd. 保留所有权利。
 *
 * 此代码可用于扩展Burp Suite社区版和Burp Suite专业版的功能，
 * 前提是该使用不违反这些产品的许可条款。
 */

package burp.api.montoya.proxy.http;

import burp.api.montoya.core.Annotations;
import burp.api.montoya.core.ByteArray;
import burp.api.montoya.core.Marker;
import burp.api.montoya.http.message.Cookie;
import burp.api.montoya.http.message.HttpHeader;
import burp.api.montoya.http.message.MimeType;
import burp.api.montoya.http.message.StatusCodeClass;
import burp.api.montoya.http.message.requests.HttpRequest;
import burp.api.montoya.http.message.responses.HttpResponse;
import burp.api.montoya.http.message.responses.analysis.Attribute;
import burp.api.montoya.http.message.responses.analysis.AttributeType;
import burp.api.montoya.http.message.responses.analysis.KeywordCount;

import java.net.InetAddress;
import java.util.List;
import java.util.regex.Pattern;

/**
 * 被Burp Proxy拦截的HTTP响应。
 */
public interface InterceptedResponse extends InterceptedHttpMessage, HttpResponse
{
    /**
     * @return 触发此响应的HTTP请求。
     * @see InterceptedResponse#initiatingRequest()
     */
    HttpRequest request();

    /**
     * @return 触发此响应的HTTP请求。
     */
    HttpRequest initiatingRequest();

    /**
     * @return 请求/响应的注释信息。
     */
    Annotations annotations();

    /**
     * 获取响应中包含的HTTP状态码。
     *
     * @return HTTP状态码。
     */
    @Override
    short statusCode();

    /**
     * 获取HTTP 1.x消息响应行中的原因短语。
     * HTTP 2消息将基于状态码返回映射的短语。
     *
     * @return HTTP原因短语。
     */
    @Override
    String reasonPhrase();

    /**
     * 测试状态码是否属于指定类别。
     *
     * @param statusCodeClass 要测试的状态码类别。
     *
     * @return 如果状态码属于该类别则返回true。
     */
    @Override
    boolean isStatusCodeClass(StatusCodeClass statusCodeClass);

    /**
     * 从HTTP 1.x消息的响应行中解析的HTTP版本。
     * HTTP 2消息将返回"HTTP/2"。
     *
     * @return HTTP版本字符串。
     */
    @Override
    String httpVersion();

    /**
     * 获取消息中的HTTP头信息。
     *
     * @return HTTP头列表。
     */
    @Override
    List<HttpHeader> headers();

    /**
     * 检查响应中是否包含指定的头信息。
     *
     * @param header 要检查的头信息。
     *
     * @return 如果包含该头信息则返回true。
     */
    @Override
    boolean hasHeader(HttpHeader header);

    /**
     * @param name 要查询的头名称。
     *
     * @return 如果响应中包含该名称的头信息则返回true。
     */
    @Override
    boolean hasHeader(String name);

    /**
     * @param name  要检查的头名称。
     * @param value 要检查的头值。
     *
     * @return 如果响应中包含匹配名称和值的头信息则返回true。
     */
    @Override
    boolean hasHeader(String name, String value);

    /**
     * @param name 要获取的头名称。
     *
     * @return 匹配名称的 {@link HttpHeader} 实例，未找到则返回 {@code null}。
     */
    @Override
    HttpHeader header(String name);

    /**
     * @param name 要获取的头名称。
     *
     * @return 匹配名称的头值字符串，未找到则返回 {@code null}。
     */
    @Override
    String headerValue(String name);

    /**
     * 获取消息体字节数组。
     *
     * @return 消息体字节数组。
     */
    @Override
    ByteArray body();

    /**
     * 获取消息体字符串。
     *
     * @return 消息体字符串。
     */
    @Override
    String bodyToString();

    /**
     * 获取消息体中消息正文的起始偏移量。
     *
     * @return 消息正文偏移量。
     */
    @Override
    int bodyOffset();

    /**
     * 获取消息标记。
     *
     * @return 标记列表。
     */
    @Override
    List<Marker> markers();

    /**
     * 获取响应中设置的HTTP Cookie。
     *
     * @return 表示响应中设置的Cookie的 {@link Cookie} 对象列表。
     */
    @Override
    List<Cookie> cookies();

    /**
     * @param name 要查找的Cookie名称。
     *
     * @return 匹配名称的 {@link Cookie} 实例，未找到则返回 {@code null}。
     */
    @Override
    Cookie cookie(String name);

    /**
     * @param name 要获取值的Cookie名称。
     *
     * @return 匹配名称的Cookie值，未找到则返回 {@code null}。
     */
    @Override
    String cookieValue(String name);

    /**
     * @param name 要检查是否存在的Cookie名称。
     *
     * @return 如果响应中包含该名称的Cookie则返回true。
     */
    @Override
    boolean hasCookie(String name);

    /**
     * @param cookie 要检查是否存在的 {@link Cookie} 实例。
     *
     * @return 如果响应中包含匹配的Cookie则返回true。
     */
    @Override
    boolean hasCookie(Cookie cookie);

    /**
     * 获取Burp Suite确定的响应MIME类型。
     *
     * @return MIME类型。
     */
    @Override
    MimeType mimeType();

    /**
     * 获取HTTP头中声明的响应MIME类型。
     *
     * @return 声明的MIME类型。
     */
    @Override
    MimeType statedMimeType();

    /**
     * 获取从HTTP消息正文内容推断的MIME类型。
     *
     * @return 推断的MIME类型。
     */
    @Override
    MimeType inferredMimeType();

    /**
     * 获取指定关键词在响应中出现的次数。
     *
     * @param keywords 要统计的关键词。
     *
     * @return 按提供顺序排列的关键词计数列表。
     */
    @Override
    List<KeywordCount> keywordCounts(String... keywords);

    /**
     * 获取响应属性的值。
     *
     * @param types 要获取值的响应属性类型。
     *
     * @return {@link Attribute} 对象列表。
     */
    @Override
    List<Attribute> attributes(AttributeType... types);

    /**
     * 在HTTP消息数据中搜索指定的搜索词。
     *
     * @param searchTerm    要搜索的值。
     * @param caseSensitive 指定搜索是否区分大小写。
     *
     * @return 如果找到搜索词则返回true。
     */
    @Override
    boolean contains(String searchTerm, boolean caseSensitive);

    /**
     * 在HTTP消息数据中搜索指定的正则表达式。
     *
     * @param pattern 要搜索的正则表达式。
     *
     * @return 如果匹配到模式则返回true。
     */
    @Override
    boolean contains(Pattern pattern);

    /**
     * 获取消息字节数组。
     *
     * @return 消息字节数组。
     */
    @Override
    ByteArray toByteArray();

    /**
     * 获取消息字符串。
     *
     * @return 消息字符串。
     */
    @Override
    String toString();

    /**
     * 创建使用新状态码的 {@code HttpResponse} 副本。
     *
     * @param statusCode 新的状态码。
     *
     * @return 新的 {@code HttpResponse} 实例。
     */
    @Override
    HttpResponse withStatusCode(short statusCode);

    /**
     * 创建使用新原因短语的 {@code HttpResponse} 副本。
     *
     * @param reasonPhrase 新的原因短语。
     *
     * @return 新的 {@code HttpResponse} 实例。
     */
    @Override
    HttpResponse withReasonPhrase(String reasonPhrase);

    /**
     * 创建使用新HTTP版本的 {@code HttpResponse} 副本。
     *
     * @param httpVersion 新的HTTP版本。
     *
     * @return 新的 {@code HttpResponse} 实例。
     */
    @Override
    HttpResponse withHttpVersion(String httpVersion);

    /**
     * 创建更新正文后的 {@code HttpResponse} 副本。<br>
     * 同时更新Content-Length头。
     *
     * @param body 响应新正文。
     *
     * @return 新的 {@code HttpResponse} 实例。
     */
    @Override
    HttpResponse withBody(String body);

    /**
     * 创建更新正文后的 {@code HttpResponse} 副本。<br>
     * 同时更新Content-Length头。
     *
     * @param body 响应新正文字节数组。
     *
     * @return 新的 {@code HttpResponse} 实例。
     */
    @Override
    HttpResponse withBody(ByteArray body);

    /**
     * 创建添加头信息后的 {@code HttpResponse} 副本。
     *
     * @param header 要添加的 {@link HttpHeader}。
     *
     * @return 添加头信息后的响应。
     */
    @Override
    HttpResponse withAddedHeader(HttpHeader header);

    /**
     * 创建添加头信息后的 {@code HttpResponse} 副本。
     *
     * @param name  头名称。
     * @param value 头值。
     *
     * @return 添加头信息后的响应。
     */
    @Override
    HttpResponse withAddedHeader(String name, String value);

    /**
     * 创建更新头信息后的 {@code HttpResponse} 副本。
     *
     * @param header 包含新值的 {@link HttpHeader}。
     *
     * @return 更新头信息后的响应。
     */
    @Override
    HttpResponse withUpdatedHeader(HttpHeader header);

    /**
     * 创建更新头信息后的 {@code HttpResponse} 副本。
     *
     * @param name  要更新的头名称。
     * @param value 指定HTTP头的新值。
     *
     * @return 更新头信息后的响应。
     */
    @Override
    HttpResponse withUpdatedHeader(String name, String value);

    /**
     * 创建移除头信息后的 {@code HttpResponse} 副本。
     *
     * @param header 要从响应中移除的 {@link HttpHeader}。
     *
     * @return 移除头信息后的响应。
     */
    @Override
    HttpResponse withRemovedHeader(HttpHeader header);

    /**
     * 创建移除头信息后的 {@code HttpResponse} 副本。
     *
     * @param name 要从响应中移除的HTTP头名称。
     *
     * @return 移除头信息后的响应。
     */
    @Override
    HttpResponse withRemovedHeader(String name);

    /**
     * 创建添加标记后的 {@code HttpResponse} 副本。
     *
     * @param markers 要添加的标记。
     *
     * @return 新的 {@code MarkedHttpRequestResponse} 实例。
     */
    @Override
    HttpResponse withMarkers(List<Marker> markers);

    /**
     * 创建添加标记后的 {@code HttpResponse} 副本。
     *
     * @param markers 要添加的标记。
     *
     * @return 新的 {@code MarkedHttpRequestResponse} 实例。
     */
    @Override
    HttpResponse withMarkers(Marker... markers);

    /**
     * 获取此请求/响应对的唯一标识符。
     *
     * @return 唯一标识单个请求/响应对的ID。
     * 扩展可以使用此ID来关联请求和响应的详细信息，
     * 并据此对响应消息进行相应处理。
     */
    @Override
    int messageId();

    /**
     * 获取处理此拦截消息的Burp Proxy监听器名称。
     *
     * @return 处理拦截消息的Burp Proxy监听器名称。
     * 格式与Proxy监听器UI中显示的相同，例如"127.0.0.1:8080"。
     */
    @Override
    String listenerInterface();

    /**
     * 获取拦截消息的源IP地址。
     *
     * @return 拦截消息的源IP地址。
     */
    @Override
    InetAddress sourceIpAddress();

    /**
     * 获取拦截消息的目标IP地址。
     *
     * @return 拦截消息的目标IP地址。
     */
    @Override
    InetAddress destinationIpAddress();
}
```
#### ProxyRequestHandler
```java
/*
 * 版权所有 (c) 2022-2023。PortSwigger Ltd. 保留所有权利。
 *
 * 此代码可用于扩展Burp Suite社区版和Burp Suite专业版的功能，
 * 前提是该使用不违反这些产品的许可条款。
 */

package burp.api.montoya.proxy.http;

import burp.api.montoya.proxy.Proxy;

/**
 * 扩展可以实现此接口，然后调用
 * {@link Proxy#registerRequestHandler(ProxyRequestHandler)} 来注册一个
 * 代理请求处理器。该处理器将会收到Proxy工具处理的请求通知。
 * 扩展可以对这些消息执行自定义分析或修改，并控制UI中的消息拦截。
 */
public interface ProxyRequestHandler
{
    /**
     * 此方法在HTTP请求被Proxy接收前调用。<br>
     * 可以修改请求。<br>
     * 可以修改注释。<br>
     * 可以控制是否拦截请求并显示给用户进行手动审查或修改。<br>
     * 可以丢弃请求。<br>
     *
     * @param interceptedRequest 一个 {@link InterceptedRequest} 对象，
     *                           扩展可以用它来查询和更新请求的详细信息。
     *
     * @return 包含所需操作、注释和要通过代理传递的HTTP请求的 {@link ProxyRequestReceivedAction}。
     */
    ProxyRequestReceivedAction handleRequestReceived(InterceptedRequest interceptedRequest);

    /**
     * 此方法在HTTP请求被Proxy处理后、发送前调用。<br>
     * 可以修改请求。<br>
     * 可以修改注释。<br>
     * 可以控制请求是发送还是丢弃。<br>
     *
     * @param interceptedRequest 一个 {@link InterceptedRequest} 对象，
     *                           扩展可以用它来查询和更新拦截请求的详细信息。
     *
     * @return 包含所需操作、注释和要从代理发送的HTTP请求的 {@link ProxyRequestToBeSentAction}。
     */
    ProxyRequestToBeSentAction handleRequestToBeSent(InterceptedRequest interceptedRequest);
}
```
#### ProxyRequestReceivedAction
```java
/*
 * 版权所有 (c) 2022-2023。PortSwigger Ltd. 保留所有权利。
 *
 * 此代码可用于扩展Burp Suite社区版和Burp Suite专业版的功能，
 * 前提是该使用不违反这些产品的许可条款。
 */

package burp.api.montoya.proxy.http;

import burp.api.montoya.core.Annotations;
import burp.api.montoya.http.message.requests.HttpRequest;
import burp.api.montoya.proxy.MessageReceivedAction;

import static burp.api.montoya.internal.ObjectFactoryLocator.FACTORY;

/**
 * 扩展在从 {@link ProxyRequestHandler#handleRequestReceived(InterceptedRequest)} 返回结果时可实现此接口。
 */
public interface ProxyRequestReceivedAction
{
    /**
     * 获取当前的初始拦截动作。
     *
     * @return {@link MessageReceivedAction} 实例
     */
    MessageReceivedAction action();

    /**
     * 获取经过扩展修改后要转发的HTTP请求。
     *
     * @return 修改后的 {@link HttpRequest} 实例
     */
    HttpRequest request();

    /**
     * 获取经过扩展修改后的当前请求的注释信息。
     *
     * @return 拦截的HTTP请求的 {@link Annotations} 实例
     */
    Annotations annotations();

    /**
     * 创建一个结果，使Burp Proxy遵循当前拦截规则决定对请求采取的操作。<br>
     * 注释信息不会被修改。
     *
     * @param request 经过扩展修改后的 {@link HttpRequest}
     * @return 遵循用户规则的 {@link ProxyRequestReceivedAction} 实例
     */
    static ProxyRequestReceivedAction continueWith(HttpRequest request)
    {
        return FACTORY.requestInitialInterceptResultFollowUserRules(request);
    }

    /**
     * 创建一个结果，使Burp Proxy遵循当前拦截规则决定对请求采取的操作。
     *
     * @param request 经过扩展修改后的 {@link HttpRequest}
     * @param annotations 拦截的HTTP请求的 {@link Annotations}
     * @return 遵循用户规则的 {@link ProxyRequestReceivedAction} 实例
     */
    static ProxyRequestReceivedAction continueWith(HttpRequest request, Annotations annotations)
    {
        return FACTORY.requestInitialInterceptResultFollowUserRules(request, annotations);
    }

    /**
     * 创建一个结果，使Burp Proxy将请求提交给用户进行手动审查或修改。<br>
     * 注释信息不会被修改。
     *
     * @param request 经过扩展修改后的 {@link HttpRequest}
     * @return 需要用户干预的 {@link ProxyRequestReceivedAction} 实例
     */
    static ProxyRequestReceivedAction intercept(HttpRequest request)
    {
        return FACTORY.requestInitialInterceptResultIntercept(request);
    }

    /**
     * 创建一个结果，使Burp Proxy将请求提交给用户进行手动审查或修改。
     *
     * @param request 经过扩展修改后的 {@link HttpRequest}
     * @param annotations 拦截的HTTP请求的 {@link Annotations}
     * @return 需要用户干预的 {@link ProxyRequestReceivedAction} 实例
     */
    static ProxyRequestReceivedAction intercept(HttpRequest request, Annotations annotations)
    {
        return FACTORY.requestInitialInterceptResultIntercept(request, annotations);
    }

    /**
     * 创建一个结果，使Burp Proxy直接转发请求而不提交给用户。<br>
     * 注释信息不会被修改。
     *
     * @param request 经过扩展修改后的 {@link HttpRequest}
     * @return 直接转发的 {@link ProxyRequestReceivedAction} 实例
     */
    static ProxyRequestReceivedAction doNotIntercept(HttpRequest request)
    {
        return FACTORY.requestInitialInterceptResultDoNotIntercept(request);
    }

    /**
     * 创建一个结果，使Burp Proxy直接转发请求而不提交给用户。
     *
     * @param request 经过扩展修改后的 {@link HttpRequest}
     * @param annotations 拦截的HTTP请求的 {@link Annotations}
     * @return 直接转发的 {@link ProxyRequestReceivedAction} 实例
     */
    static ProxyRequestReceivedAction doNotIntercept(HttpRequest request, Annotations annotations)
    {
        return FACTORY.requestInitialInterceptResultDoNotIntercept(request, annotations);
    }

    /**
     * 创建一个结果，使Burp Proxy丢弃该请求。
     *
     * @return 丢弃请求的 {@link ProxyRequestReceivedAction} 实例
     */
    static ProxyRequestReceivedAction drop()
    {
        return FACTORY.requestInitialInterceptResultDrop();
    }

    /**
     * 创建HTTP请求初始拦截结果的默认实现。
     *
     * @param request 经过扩展修改后的 {@link HttpRequest}
     * @param annotations 拦截的HTTP请求的 {@link Annotations}，为null则保持原样
     * @param action HTTP请求的 {@link MessageReceivedAction}
     * @return 包含HTTP请求、注释和初始拦截动作的 {@link ProxyRequestReceivedAction} 实例
     */
    static ProxyRequestReceivedAction proxyRequestReceivedAction(HttpRequest request, Annotations annotations, MessageReceivedAction action)
    {
        return FACTORY.proxyRequestReceivedAction(request, annotations, action);
    }
}
```
#### ProxyRequestToBeSentAction
```java
/*
 * 版权所有 (c) 2022-2023。PortSwigger Ltd. 保留所有权利。
 *
 * 此代码可用于扩展Burp Suite社区版和Burp Suite专业版的功能，
 * 前提是该使用不违反这些产品的许可条款。
 */

package burp.api.montoya.proxy.http;

import burp.api.montoya.core.Annotations;
import burp.api.montoya.http.message.requests.HttpRequest;
import burp.api.montoya.proxy.MessageToBeSentAction;

import static burp.api.montoya.internal.ObjectFactoryLocator.FACTORY;

/**
 * 扩展在从 {@link ProxyRequestHandler#handleRequestToBeSent(InterceptedRequest)} 返回结果时可实现此接口。
 */
public interface ProxyRequestToBeSentAction
{
    /**
     * 获取当前的最终拦截动作。
     *
     * @return {@link MessageToBeSentAction} 实例
     */
    MessageToBeSentAction action();

    /**
     * 获取经过扩展修改后要转发的HTTP请求。
     *
     * @return 修改后的 {@link HttpRequest} 实例
     */
    HttpRequest request();

    /**
     * 获取经过扩展修改后的当前请求的注释信息。
     *
     * @return 拦截的HTTP请求的 {@link Annotations} 实例
     */
    Annotations annotations();

    /**
     * 创建一个结果，使Burp Proxy转发该请求。<br>
     * 注释信息不会被修改。
     *
     * @param request 经过扩展修改后的 {@link HttpRequest}
     * @return 转发请求的 {@link ProxyRequestToBeSentAction} 实例
     */
    static ProxyRequestToBeSentAction continueWith(HttpRequest request)
    {
        return FACTORY.requestFinalInterceptResultContinueWith(request);
    }

    /**
     * 创建一个结果，使Burp Proxy转发该请求。
     *
     * @param request 经过扩展修改后的 {@link HttpRequest}
     * @param annotations 拦截的HTTP请求的 {@link Annotations}
     * @return 转发请求的 {@link ProxyRequestToBeSentAction} 实例
     */
    static ProxyRequestToBeSentAction continueWith(HttpRequest request, Annotations annotations)
    {
        return FACTORY.requestFinalInterceptResultContinueWith(request, annotations);
    }

    /**
     * 创建一个结果，使Burp Proxy丢弃该请求。
     *
     * @return 丢弃请求的 {@link ProxyRequestToBeSentAction} 实例
     */
    static ProxyRequestToBeSentAction drop()
    {
        return FACTORY.requestFinalInterceptResultDrop();
    }

    /**
     * 创建HTTP请求最终拦截结果的默认实现。
     *
     * @param request 经过扩展修改后的 {@link HttpRequest}
     * @param annotations 拦截的HTTP请求的 {@link Annotations}，为null则保持原样
     * @param action HTTP请求的 {@link MessageToBeSentAction}
     * @return 包含HTTP请求、注释和最终拦截动作的 {@link ProxyRequestToBeSentAction} 实例
     */
    static ProxyRequestToBeSentAction proxyRequestToBeSentAction(HttpRequest request, Annotations annotations, MessageToBeSentAction action)
    {
        return FACTORY.proxyRequestToBeSentAction(request, annotations, action);
    }
}
```
#### ProxyResponseHandler
```java
/*
 * 版权所有 (c) 2022-2023。PortSwigger Ltd. 保留所有权利。
 *
 * 此代码可用于扩展Burp Suite社区版和Burp Suite专业版的功能，
 * 前提是该使用不违反这些产品的许可条款。
 */

package burp.api.montoya.proxy.http;

import burp.api.montoya.proxy.Proxy;

/**
 * 扩展可以实现此接口，然后调用
 * {@link Proxy#registerResponseHandler(ProxyResponseHandler)} 来注册一个
 * 代理响应处理器。该处理器将会收到Proxy工具处理的响应通知。
 * 扩展可以对这些响应执行自定义分析或修改，并控制UI中的消息拦截。
 */
public interface ProxyResponseHandler
{
    /**
     * 当Proxy接收到HTTP响应时调用此方法。
     *
     * @param interceptedResponse 一个 {@link InterceptedResponse} 对象，
     *                            扩展可以用它来查询和更新响应的详细信息，
     *                            并控制是否拦截响应并显示给用户进行手动审查或修改。
     *
     * @return 包含所需操作、HTTP响应和注释的 {@link ProxyResponseReceivedAction}，
     *         这些信息将被传递下去。
     */
    ProxyResponseReceivedAction handleResponseReceived(InterceptedResponse interceptedResponse);

    /**
     * 当Proxy处理完HTTP响应但还未返回给客户端时调用此方法。
     *
     * @param interceptedResponse 一个 {@link InterceptedResponse} 对象，
     *                            扩展可以用它来查询和更新响应的详细信息。
     *
     * @return 包含所需操作、HTTP响应和注释的 {@link ProxyResponseToBeSentAction}，
     *         这些信息将被传递下去。
     */
    ProxyResponseToBeSentAction handleResponseToBeSent(InterceptedResponse interceptedResponse);
}
```
#### ProxyResponseReceivedAction
```java
/*
 * 版权所有 (c) 2022-2023。PortSwigger Ltd. 保留所有权利。
 *
 * 此代码可用于扩展Burp Suite社区版和Burp Suite专业版的功能，
 * 前提是该使用不违反这些产品的许可条款。
 */

package burp.api.montoya.proxy.http;

import burp.api.montoya.core.Annotations;
import burp.api.montoya.http.message.responses.HttpResponse;
import burp.api.montoya.proxy.MessageReceivedAction;

import static burp.api.montoya.internal.ObjectFactoryLocator.FACTORY;

/**
 * 扩展在从 {@link ProxyResponseHandler#handleResponseReceived(InterceptedResponse)} 返回结果时可实现此接口。
 */
public interface ProxyResponseReceivedAction
{
    /**
     * 获取当前的初始拦截动作。
     *
     * @return {@link MessageReceivedAction} 实例
     */
    MessageReceivedAction action();

    /**
     * 获取经过扩展修改后要转发的HTTP响应。
     *
     * @return 修改后的 {@link HttpResponse} 实例
     */
    HttpResponse response();

    /**
     * 获取经过扩展修改后的当前响应的注释信息。
     *
     * @return 拦截的HTTP响应的 {@link Annotations} 实例
     */
    Annotations annotations();

    /**
     * 创建一个动作，使Burp Proxy遵循当前拦截规则决定对响应采取的操作。<br>
     * 注释信息不会被修改。
     *
     * @param response 经过扩展修改后的 {@link HttpResponse}
     * @return 遵循用户规则的 {@link ProxyResponseReceivedAction} 实例
     */
    static ProxyResponseReceivedAction continueWith(HttpResponse response)
    {
        return FACTORY.responseInitialInterceptResultFollowUserRules(response);
    }

    /**
     * 创建一个动作，使Burp Proxy遵循当前拦截规则决定对响应采取的操作。
     *
     * @param response 经过扩展修改后的 {@link HttpResponse}
     * @param annotations 拦截的HTTP响应的 {@link Annotations}
     * @return 遵循用户规则的 {@link ProxyResponseReceivedAction} 实例
     */
    static ProxyResponseReceivedAction continueWith(HttpResponse response, Annotations annotations)
    {
        return FACTORY.responseInitialInterceptResultFollowUserRules(response, annotations);
    }

    /**
     * 创建一个动作，使Burp Proxy将响应提交给用户进行手动审查或修改。<br>
     * 注释信息不会被修改。
     *
     * @param response 经过扩展修改后的 {@link HttpResponse}
     * @return 需要用户干预的 {@link ProxyResponseReceivedAction} 实例
     */
    static ProxyResponseReceivedAction intercept(HttpResponse response)
    {
        return FACTORY.responseInitialInterceptResultIntercept(response);
    }

    /**
     * 创建一个动作，使Burp Proxy将响应提交给用户进行手动审查或修改。
     *
     * @param response 经过扩展修改后的 {@link HttpResponse}
     * @param annotations 拦截的HTTP响应的 {@link Annotations}
     * @return 需要用户干预的 {@link ProxyResponseReceivedAction} 实例
     */
    static ProxyResponseReceivedAction intercept(HttpResponse response, Annotations annotations)
    {
        return FACTORY.responseInitialInterceptResultIntercept(response, annotations);
    }

    /**
     * 创建一个动作，使Burp Proxy直接转发响应而不提交给用户。<br>
     * 注释信息不会被修改。
     *
     * @param response 经过扩展修改后的 {@link HttpResponse}
     * @return 直接转发的 {@link ProxyResponseReceivedAction} 实例
     */
    static ProxyResponseReceivedAction doNotIntercept(HttpResponse response)
    {
        return FACTORY.responseInitialInterceptResultDoNotIntercept(response);
    }

    /**
     * 创建一个动作，使Burp Proxy直接转发响应而不提交给用户。
     *
     * @param response 经过扩展修改后的 {@link HttpResponse}
     * @param annotations 拦截的HTTP响应的 {@link Annotations}
     * @return 直接转发的 {@link ProxyResponseReceivedAction} 实例
     */
    static ProxyResponseReceivedAction doNotIntercept(HttpResponse response, Annotations annotations)
    {
        return FACTORY.responseInitialInterceptResultDoNotIntercept(response, annotations);
    }

    /**
     * 创建一个动作，使Burp Proxy丢弃该响应。
     *
     * @return 丢弃响应的 {@link ProxyResponseReceivedAction} 实例
     */
    static ProxyResponseReceivedAction drop()
    {
        return FACTORY.responseInitialInterceptResultDrop();
    }

    /**
     * 创建HTTP响应初始拦截结果的默认实现。
     *
     * @param response 经过扩展修改后的 {@link HttpResponse}
     * @param annotations 拦截的HTTP响应的 {@link Annotations}，为null则保持原样
     * @param action HTTP响应的 {@link MessageReceivedAction}
     * @return 包含HTTP响应、注释和拦截动作的 {@link ProxyResponseReceivedAction} 实例
     */
    static ProxyResponseReceivedAction proxyResponseReceivedAction(HttpResponse response, Annotations annotations, MessageReceivedAction action)
    {
        return FACTORY.proxyResponseReceivedAction(response, annotations, action);
    }
}
```
#### ProxyResponseToBeSentAction
```java
/*
 * 版权所有 (c) 2022-2023。PortSwigger Ltd. 保留所有权利。
 *
 * 此代码可用于扩展Burp Suite社区版和Burp Suite专业版的功能，
 * 前提是该使用不违反这些产品的许可条款。
 */

package burp.api.montoya.proxy.http;

import burp.api.montoya.core.Annotations;
import burp.api.montoya.http.message.responses.HttpResponse;
import burp.api.montoya.proxy.MessageToBeSentAction;

import static burp.api.montoya.internal.ObjectFactoryLocator.FACTORY;

/**
 * 扩展在从 {@link ProxyResponseHandler#handleResponseToBeSent(InterceptedResponse)} 
 * 返回结果时可实现此接口。
 */
public interface ProxyResponseToBeSentAction
{
    /**
     * 获取当前的最终拦截动作。
     *
     * @return {@link MessageToBeSentAction} 实例
     */
    MessageToBeSentAction action();

    /**
     * 获取经过扩展修改后要转发的HTTP响应。
     *
     * @return 修改后的 {@link HttpResponse} 实例
     */
    HttpResponse response();

    /**
     * 获取经过扩展修改后的当前响应的注释信息。
     *
     * @return 拦截的HTTP响应的 {@link Annotations} 实例
     */
    Annotations annotations();

    /**
     * 创建一个结果，使Burp Proxy转发该响应。<br>
     * 注释信息不会被修改。
     *
     * @param response 经过扩展修改后的 {@link HttpResponse}
     * @return 转发响应的 {@link ProxyResponseToBeSentAction} 实例
     */
    static ProxyResponseToBeSentAction continueWith(HttpResponse response)
    {
        return FACTORY.responseFinalInterceptResultContinueWith(response);
    }

    /**
     * 创建一个结果，使Burp Proxy转发该响应。
     *
     * @param response 经过扩展修改后的 {@link HttpResponse}
     * @param annotations 拦截的HTTP响应的 {@link Annotations}
     * @return 转发响应的 {@link ProxyResponseToBeSentAction} 实例
     */
    static ProxyResponseToBeSentAction continueWith(HttpResponse response, Annotations annotations)
    {
        return FACTORY.responseFinalInterceptResultContinueWith(response, annotations);
    }

    /**
     * 创建一个结果，使Burp Proxy丢弃该响应。
     *
     * @return 丢弃响应的 {@link ProxyResponseToBeSentAction} 实例
     */
    static ProxyResponseToBeSentAction drop()
    {
        return FACTORY.responseFinalInterceptResultDrop();
    }

    /**
     * 创建HTTP响应最终拦截结果的默认实现。
     *
     * @param response 经过扩展修改后的 {@link HttpResponse}
     * @param annotations 拦截的HTTP响应的 {@link Annotations}，为null则保持原样
     * @param action HTTP响应的 {@link MessageToBeSentAction}
     * @return 包含HTTP响应、注释和最终拦截动作的 {@link ProxyResponseToBeSentAction} 实例
     */
    static ProxyResponseToBeSentAction proxyResponseToReturnAction(HttpResponse response, Annotations annotations, MessageToBeSentAction action)
    {
        return FACTORY.proxyResponseToReturnAction(response, annotations, action);
    }
}
```
### websocket
#### BinaryMessageReceivedAction
```java
/*
 * 版权所有 (c) 2022-2023。PortSwigger Ltd. 保留所有权利。
 *
 * 此代码可用于扩展Burp Suite社区版和Burp Suite专业版的功能，
 * 前提是该使用不违反这些产品的许可条款。
 */

package burp.api.montoya.proxy.websocket;

import burp.api.montoya.core.ByteArray;
import burp.api.montoya.proxy.MessageReceivedAction;
import burp.api.montoya.websocket.BinaryMessage;

import static burp.api.montoya.internal.ObjectFactoryLocator.FACTORY;

/**
 * 扩展在从 {@link ProxyMessageHandler#handleBinaryMessageReceived(InterceptedBinaryMessage)} 
 * 返回二进制消息时可实现此接口。
 */
public interface BinaryMessageReceivedAction
{
    /**
     * @return 与此消息关联的动作
     */
    MessageReceivedAction action();

    /**
     * @return 此消息的有效载荷
     */
    ByteArray payload();

    /**
     * 构建一个二进制WebSocket消息，
     * 遵循当前拦截规则决定对消息采取的适当操作。
     *
     * @param payload 二进制消息有效载荷
     * @return 允许遵循用户规则的 {@link BinaryMessageReceivedAction}
     */
    static BinaryMessageReceivedAction continueWith(ByteArray payload)
    {
        return FACTORY.followUserRulesInitialProxyBinaryMessage(payload);
    }

    /**
     * 构建一个二进制WebSocket消息，
     * 遵循当前拦截规则决定对消息采取的适当操作。
     *
     * @param message 二进制消息
     * @return 允许遵循用户规则的 {@link BinaryMessageReceivedAction}
     */
    static BinaryMessageReceivedAction continueWith(BinaryMessage message)
    {
        return FACTORY.followUserRulesInitialProxyBinaryMessage(message.payload());
    }

    /**
     * 构建一个要在Proxy中拦截的二进制WebSocket消息。
     *
     * @param payload 二进制消息有效载荷
     * @return 要拦截的消息
     */
    static BinaryMessageReceivedAction intercept(ByteArray payload)
    {
        return FACTORY.interceptInitialProxyBinaryMessage(payload);
    }

    /**
     * 构建一个要在Proxy中拦截的二进制WebSocket消息。
     *
     * @param message 二进制消息
     * @return 要拦截的消息
     */
    static BinaryMessageReceivedAction intercept(BinaryMessage message)
    {
        return FACTORY.interceptInitialProxyBinaryMessage(message.payload());
    }

    /**
     * 构建一个要在Proxy中继续传输而不被拦截的二进制WebSocket消息。
     *
     * @param payload 二进制消息有效载荷
     * @return 不被拦截的消息
     */
    static BinaryMessageReceivedAction doNotIntercept(ByteArray payload)
    {
        return FACTORY.doNotInterceptInitialProxyBinaryMessage(payload);
    }

    /**
     * 构建一个要在Proxy中继续传输而不被拦截的二进制WebSocket消息。
     *
     * @param message 二进制消息
     * @return 不被拦截的消息
     */
    static BinaryMessageReceivedAction doNotIntercept(BinaryMessage message)
    {
        return FACTORY.doNotInterceptInitialProxyBinaryMessage(message.payload());
    }

    /**
     * 构建一个要被丢弃的二进制WebSocket消息。
     *
     * @return 要被丢弃的消息
     */
    static BinaryMessageReceivedAction drop()
    {
        return FACTORY.dropInitialProxyBinaryMessage();
    }
}
```
#### BinaryMessageToBeSentAction
```java
/*
 * 版权所有 (c) 2022-2023。PortSwigger Ltd. 保留所有权利。
 *
 * 此代码可用于扩展Burp Suite社区版和Burp Suite专业版的功能，
 * 前提是该使用不违反这些产品的许可条款。
 */

package burp.api.montoya.proxy.websocket;

import burp.api.montoya.core.ByteArray;
import burp.api.montoya.proxy.MessageToBeSentAction;
import burp.api.montoya.websocket.BinaryMessage;

import static burp.api.montoya.internal.ObjectFactoryLocator.FACTORY;

/**
 * 扩展在从 {@link ProxyMessageHandler#handleBinaryMessageToBeSent(InterceptedBinaryMessage)} 
 * 返回二进制消息时可实现此接口。
 */
public interface BinaryMessageToBeSentAction
{
    /**
     * @return 与此消息关联的动作
     */
    MessageToBeSentAction action();

    /**
     * @return 此消息的有效载荷
     */
    ByteArray payload();

    /**
     * 构建一个要通过Burp继续传输的二进制WebSocket消息。
     *
     * @param payload 二进制消息有效载荷
     * @return 要继续传输的消息
     */
    static BinaryMessageToBeSentAction continueWith(ByteArray payload)
    {
        return FACTORY.continueWithFinalProxyBinaryMessage(payload);
    }

    /**
     * 构建一个要通过Burp继续传输的二进制WebSocket消息。
     *
     * @param message 二进制消息
     * @return 要继续传输的消息
     */
    static BinaryMessageToBeSentAction continueWith(BinaryMessage message)
    {
        return FACTORY.continueWithFinalProxyBinaryMessage(message.payload());
    }

    /**
     * 构建一个要被丢弃的二进制WebSocket消息。
     *
     * @return 要被丢弃的消息
     */
    static BinaryMessageToBeSentAction drop()
    {
        return FACTORY.dropFinalProxyBinaryMessage();
    }
}
```
#### InterceptedBinaryMessage
```java
/*
 * 版权所有 (c) 2022-2023。PortSwigger Ltd. 保留所有权利。
 *
 * 此代码可用于扩展Burp Suite社区版和Burp Suite专业版的功能，
 * 前提是该使用不违反这些产品的许可条款。
 */

package burp.api.montoya.proxy.websocket;

import burp.api.montoya.core.Annotations;
import burp.api.montoya.core.ByteArray;
import burp.api.montoya.websocket.BinaryMessage;
import burp.api.montoya.websocket.Direction;

/**
 * 被Burp Proxy拦截的二进制WebSocket消息。
 */
public interface InterceptedBinaryMessage extends BinaryMessage
{
    /**
     * @return 消息的注释信息
     */
    Annotations annotations();

    /**
     * @return 基于二进制的WebSocket消息载荷
     */
    @Override
    ByteArray payload();

    /**
     * @return 消息的传输方向
     */
    @Override
    Direction direction();
}
```
#### InterceptedTextMessage
```java
/*
 * 版权所有 (c) 2022-2023。PortSwigger Ltd. 保留所有权利。
 *
 * 此代码可用于扩展Burp Suite社区版和Burp Suite专业版的功能，
 * 前提是该使用不违反这些产品的许可条款。
 */

package burp.api.montoya.proxy.websocket;

import burp.api.montoya.core.Annotations;
import burp.api.montoya.websocket.Direction;
import burp.api.montoya.websocket.TextMessage;

/**
 * 被Burp Proxy拦截的文本格式WebSocket消息。
 */
public interface InterceptedTextMessage extends TextMessage
{
    /**
     * @return 消息的注释信息
     */
    Annotations annotations();

    /**
     * @return 基于文本的WebSocket消息内容
     */
    @Override
    String payload();

    /**
     * @return 消息的传输方向
     */
    @Override
    Direction direction();
}
```
#### ProxyMessageHandler
```java
/*
 * 版权所有 (c) 2022-2023。PortSwigger Ltd. 保留所有权利。
 *
 * 此代码可用于扩展Burp Suite社区版和Burp Suite专业版的功能，
 * 前提是该使用不违反这些产品的许可条款。
 */

package burp.api.montoya.proxy.websocket;

/**
 * 此接口允许扩展在通过代理WebSocket发送/接收消息或连接关闭时收到通知。
 */
public interface ProxyMessageHandler
{
    /**
     * 当从客户端或服务端接收到文本消息时调用。
     * 扩展可以在此修改消息内容，然后才由Burp处理。
     *
     * @param interceptedTextMessage 被拦截的文本WebSocket消息
     * @return 包含所需操作和待传递文本消息的 {@link TextMessageReceivedAction}
     */
    TextMessageReceivedAction handleTextMessageReceived(InterceptedTextMessage interceptedTextMessage);

    /**
     * 当文本消息即将发送给客户端或服务端时调用。
     * 扩展可以在此修改消息内容，然后才发送。
     *
     * @param interceptedTextMessage 被拦截的文本WebSocket消息
     * @return 包含所需操作和待传递文本消息的 {@link TextMessageToBeSentAction}
     */
    TextMessageToBeSentAction handleTextMessageToBeSent(InterceptedTextMessage interceptedTextMessage);

    /**
     * 当从客户端或服务端接收到二进制消息时调用。
     * 扩展可以在此修改消息内容，然后才由Burp处理。
     *
     * @param interceptedBinaryMessage 被拦截的二进制WebSocket消息
     * @return 包含所需操作和待传递二进制消息的 {@link BinaryMessageReceivedAction}
     */
    BinaryMessageReceivedAction handleBinaryMessageReceived(InterceptedBinaryMessage interceptedBinaryMessage);

    /**
     * 当二进制消息即将发送给客户端或服务端时调用。
     * 扩展可以在此修改消息内容，然后才发送。
     *
     * @param interceptedBinaryMessage 被拦截的二进制WebSocket消息
     * @return 包含所需操作和待传递二进制消息的 {@link BinaryMessageToBeSentAction}
     */
    BinaryMessageToBeSentAction handleBinaryMessageToBeSent(InterceptedBinaryMessage interceptedBinaryMessage);

    /**
     * 当WebSocket连接关闭时调用。
     */
    default void onClose()
    {
    }
}
```
#### ProxyWebSocket
```java
/*
 * 版权所有 (c) 2022-2023。PortSwigger Ltd. 保留所有权利。
 *
 * 此代码可用于扩展Burp Suite社区版和Burp Suite专业版的功能，
 * 前提是该使用不违反这些产品的许可条款。
 */

package burp.api.montoya.proxy.websocket;

import burp.api.montoya.core.ByteArray;
import burp.api.montoya.core.Registration;
import burp.api.montoya.websocket.Direction;

/**
 * Burp中的代理WebSocket接口。
 */
public interface ProxyWebSocket
{
    /**
     * 允许扩展通过WebSocket向客户端或服务端发送文本消息。
     *
     * @param textMessage 要发送的文本消息
     * @param direction   消息的传输方向
     */
    void sendTextMessage(String textMessage, Direction direction);

    /**
     * 允许扩展通过WebSocket向客户端或服务端发送二进制消息。
     *
     * @param binaryMessage 要发送的二进制消息
     * @param direction     消息的传输方向
     */
    void sendBinaryMessage(ByteArray binaryMessage, Direction direction);

    /**
     * 关闭WebSocket连接。
     */
    void close();

    /**
     * 注册消息处理器，用于在WebSocket收发消息时执行操作。
     *
     * @param handler 扩展实现的 {@link ProxyMessageHandler} 接口对象
     * @return 处理器的 {@link Registration} 注册对象
     */
    Registration registerProxyMessageHandler(ProxyMessageHandler handler);
}
```
#### ProxyWebSocketCreation
```java
/*
 * 版权所有 (c) 2022-2023。PortSwigger Ltd. 保留所有权利。
 *
 * 此代码可用于扩展Burp Suite社区版和Burp Suite专业版的功能，
 * 前提是该使用不违反这些产品的许可条款。
 */

package burp.api.montoya.proxy.websocket;

import burp.api.montoya.http.message.requests.HttpRequest;

/**
 * 正在创建的代理WebSocket连接相关信息。
 */
public interface ProxyWebSocketCreation
{
    /**
     * @return 正在创建的ProxyWebSocket实例
     */
    ProxyWebSocket proxyWebSocket();

    /**
     * @return 触发WebSocket创建的HTTP升级请求
     */
    HttpRequest upgradeRequest();
}
```
#### ProxyWebSocketCreationHandler
```java
/*
 * 版权所有 (c) 2022-2023。PortSwigger Ltd. 保留所有权利。
 *
 * 此代码可用于扩展Burp Suite社区版和Burp Suite专业版的功能，
 * 前提是该使用不违反这些产品的许可条款。
 */

package burp.api.montoya.proxy.websocket;

import burp.api.montoya.proxy.Proxy;

/**
 * 扩展可实现此接口并通过调用 {@link Proxy#registerWebSocketCreationHandler} 注册WebSocket处理器。<br>
 * 当Proxy工具创建新WebSocket连接时，该处理器将收到通知。
 */
public interface ProxyWebSocketCreationHandler
{
    /**
     * 当Proxy工具创建WebSocket连接时由Burp调用。<br>
     * <b>注意</b>：客户端连接将在该方法执行完成后才会升级。
     *
     * @param webSocketCreation 包含正在创建的代理WebSocket相关信息的 {@link ProxyWebSocketCreation} 对象
     */
    void handleWebSocketCreation(ProxyWebSocketCreation webSocketCreation);
}
```
#### TextMessageReceivedAction
```java
/*
 * 版权所有 (c) 2022-2023。PortSwigger Ltd. 保留所有权利。
 *
 * 此代码可用于扩展Burp Suite社区版和Burp Suite专业版的功能，
 * 前提是该使用不违反这些产品的许可条款。
 */

package burp.api.montoya.proxy.websocket;

import burp.api.montoya.proxy.MessageReceivedAction;
import burp.api.montoya.websocket.TextMessage;

import static burp.api.montoya.internal.ObjectFactoryLocator.FACTORY;

/**
 * 扩展在从 {@link ProxyMessageHandler#handleTextMessageReceived(InterceptedTextMessage)} 
 * 返回文本消息时可实现此接口。
 */
public interface TextMessageReceivedAction
{
    /**
     * @return 与此消息关联的操作
     */
    MessageReceivedAction action();

    /**
     * @return 此消息的有效载荷内容
     */
    String payload();

    /**
     * 构建文本WebSocket消息，遵循当前拦截规则决定对消息采取的操作。
     *
     * @param payload 文本消息内容
     * @return 允许遵循用户规则的 {@link TextMessageReceivedAction}
     */
    static TextMessageReceivedAction continueWith(String payload)
    {
        return FACTORY.followUserRulesInitialProxyTextMessage(payload);
    }

    /**
     * 构建文本WebSocket消息，遵循当前拦截规则决定对消息采取的操作。
     *
     * @param message 文本消息对象
     * @return 允许遵循用户规则的 {@link TextMessageReceivedAction}
     */
    static TextMessageReceivedAction continueWith(TextMessage message)
    {
        return FACTORY.followUserRulesInitialProxyTextMessage(message.payload());
    }

    /**
     * 构建要在Proxy中拦截的文本WebSocket消息。
     *
     * @param payload 文本消息内容
     * @return 要拦截的消息
     */
    static TextMessageReceivedAction intercept(String payload)
    {
        return FACTORY.interceptInitialProxyTextMessage(payload);
    }

    /**
     * 构建要在Proxy中拦截的文本WebSocket消息。
     *
     * @param message 文本消息对象
     * @return 要拦截的消息
     */
    static TextMessageReceivedAction intercept(TextMessage message)
    {
        return FACTORY.interceptInitialProxyTextMessage(message.payload());
    }

    /**
     * 构建要在Proxy中继续传输而不被拦截的文本WebSocket消息。
     *
     * @param payload 文本消息内容
     * @return 不被拦截的消息
     */
    static TextMessageReceivedAction doNotIntercept(String payload)
    {
        return FACTORY.doNotInterceptInitialProxyTextMessage(payload);
    }

    /**
     * 构建要在Proxy中继续传输而不被拦截的文本WebSocket消息。
     *
     * @param message 文本消息对象
     * @return 不被拦截的消息
     */
    static TextMessageReceivedAction doNotIntercept(TextMessage message)
    {
        return FACTORY.doNotInterceptInitialProxyTextMessage(message.payload());
    }

    /**
     * 构建要被丢弃的文本WebSocket消息。
     *
     * @return 要被丢弃的消息
     */
    static TextMessageReceivedAction drop()
    {
        return FACTORY.dropInitialProxyTextMessage();
    }
}
```
#### TextMessageToBeSentAction
```java

```
## repeater
### 
```java

```
### 
```java

```
### 
```java

```
## scanner
## scope
## sitemap
## ui
### 
```java

```
### 
```java

```
### 
```java

```
### contextmenu
#### 
```java

```
#### 
```java

```
#### 
```java

```
#### 
```java

```
#### 
```java

```
#### 
```java

```
#### 
```java

```
#### 
```java

```
#### 
```java

```
#### WebSocketMessage
```java
/*
 * 版权所有 (c) 2023。PortSwigger Ltd. 保留所有权利。
 *
 * 此代码可用于扩展Burp Suite社区版和Burp Suite专业版的功能，
 * 前提是该使用不违反这些产品的许可条款。
 */

package burp.api.montoya.ui.contextmenu;

import burp.api.montoya.core.Annotations;
import burp.api.montoya.core.ByteArray;
import burp.api.montoya.http.message.requests.HttpRequest;
import burp.api.montoya.websocket.Direction;

/**
 * 表示WebSocket消息的接口，提供对消息内容和元数据的访问。
 */
public interface WebSocketMessage
{
    /**
     * 获取消息的注释信息。
     *
     * @return 消息的{@link Annotations}注释对象
     */
    Annotations annotations();

    /**
     * @return 消息的传输方向
     */
    Direction direction();

    /**
     * @return WebSocket消息的有效载荷内容
     */
    ByteArray payload();

    /**
     * @return 用于创建WebSocket连接的原始{@link HttpRequest}请求
     */
    HttpRequest upgradeRequest();
}
```
### editor
#### Editor
```java
/*
 * 版权所有 (c) 2022-2023。PortSwigger Ltd. 保留所有权利。
 *
 * 此代码可用于扩展Burp Suite社区版和Burp Suite专业版的功能，
 * 前提是该使用不违反这些产品的许可条款。
 */

package burp.api.montoya.ui.editor;

import burp.api.montoya.ui.Selection;

import java.awt.Component;
import java.util.Optional;

/**
 * 定义不同类型编辑器之间的共享行为。
 */
public interface Editor
{
    /**
     * 更新编辑器下方搜索栏中显示的搜索表达式。
     *
     * @param expression 搜索表达式
     */
    void setSearchExpression(String expression);

    /**
     * @return 如果用户自上次编程设置内容后修改了编辑器内容，则返回true
     */
    boolean isModified();

    /**
     * @return 当前编辑器中光标位置的索引
     */
    int caretPosition();

    /**
     * 如果用户没有进行选择，则返回{@link Optional#empty()}
     *
     * @return 包含用户在编辑器中的当前选择的{@link Optional}对象
     */
    Optional<Selection> selection();

    /**
     * @return 编辑器的UI组件，供扩展程序添加到自己的用户界面中
     */
    Component uiComponent();
}
```
#### EditorOptions
```java
/*
 * 版权所有 (c) 2022-2023。PortSwigger Ltd. 保留所有权利。
 *
 * 此代码可用于扩展Burp Suite社区版和Burp Suite专业版的功能，
 * 前提是该使用不违反这些产品的许可条款。
 */

package burp.api.montoya.ui.editor;

/**
 * 这些选项允许您为{@link Editor}实现配置额外的行为。
 */
public enum EditorOptions
{
    /**
     * 编辑器应为只读模式
     */
    READ_ONLY,
    
    /**
     * 编辑器应自动换行 - 仅适用于原始编辑器(Raw Editors)
     */
    WRAP_LINES,
    
    /**
     * 编辑器应显示不可打印字符 - 仅适用于原始编辑器(Raw Editors)
     */
    SHOW_NON_PRINTABLE_CHARACTERS
}
```
#### HttpRequestEditor
```java
/*
 * 版权所有 (c) 2022-2023。PortSwigger Ltd. 保留所有权利。
 *
 * 此代码可用于扩展Burp Suite社区版和Burp Suite专业版的功能，
 * 前提是该使用不违反这些产品的许可条款。
 */

package burp.api.montoya.ui.editor;

import burp.api.montoya.http.message.requests.HttpRequest;
import burp.api.montoya.ui.Selection;

import java.awt.Component;
import java.util.Optional;

/**
 * 为扩展程序提供Burp Suite的HTTP请求编辑器实例，用于扩展程序自身的用户界面。
 */
public interface HttpRequestEditor extends Editor
{
    /**
     * @return 从编辑器内容派生的{@link HttpRequest}实例
     */
    HttpRequest getRequest();

    /**
     * 在编辑器中显示HTTP请求的内容
     *
     * @param request 要设置的HTTP请求
     */
    void setRequest(HttpRequest request);

    /**
     * 更新编辑器下方搜索栏中显示的搜索表达式
     *
     * @param expression 搜索表达式
     */
    @Override
    void setSearchExpression(String expression);

    /**
     * @return 如果用户自上次编程设置内容后修改了编辑器内容，则返回true
     */
    @Override
    boolean isModified();

    /**
     * @return 当前编辑器中光标位置的索引
     */
    @Override
    int caretPosition();

    /**
     * 如果用户没有进行选择，则返回{@link Optional#empty()}
     *
     * @return 包含用户在编辑器中的当前选择的{@link Optional}对象
     */
    @Override
    Optional<Selection> selection();

    /**
     * @return 编辑器的UI组件，供扩展程序添加到自己的用户界面中
     */
    @Override
    Component uiComponent();
}
```
#### HttpResponseEditor
```java
/*
 * 版权所有 (c) 2022-2023。PortSwigger Ltd. 保留所有权利。
 *
 * 此代码可用于扩展Burp Suite社区版和Burp Suite专业版的功能，
 * 前提是该使用不违反这些产品的许可条款。
 */

package burp.api.montoya.ui.editor;

import burp.api.montoya.http.message.responses.HttpResponse;
import burp.api.montoya.ui.Selection;

import java.awt.Component;
import java.util.Optional;

/**
 * 为扩展程序提供Burp Suite的HTTP响应编辑器实例，用于扩展程序自身的用户界面。
 */
public interface HttpResponseEditor extends Editor
{
    /**
     * @return 从编辑器内容派生的{@link HttpResponse}实例
     */
    HttpResponse getResponse();

    /**
     * 在编辑器中显示HTTP响应的内容
     *
     * @param response 要设置的HTTP响应
     */
    void setResponse(HttpResponse response);

    /**
     * 更新编辑器下方搜索栏中显示的搜索表达式
     *
     * @param expression 搜索表达式
     */
    @Override
    void setSearchExpression(String expression);

    /**
     * @return 如果用户自上次编程设置内容后修改了编辑器内容，则返回true
     */
    @Override
    boolean isModified();

    /**
     * @return 当前编辑器中光标位置的索引
     */
    @Override
    int caretPosition();

    /**
     * 如果用户没有进行选择，则返回{@link Optional#empty()}
     *
     * @return 包含用户在编辑器中的当前选择的{@link Optional}对象
     */
    @Override
    Optional<Selection> selection();

    /**
     * @return 编辑器的UI组件，供扩展程序添加到自己的用户界面中
     */
    @Override
    Component uiComponent();
}
```
#### RawEditor
```java
/*
 * 版权所有 (c) 2022-2023。PortSwigger Ltd. 保留所有权利。
 *
 * 此代码可用于扩展Burp Suite社区版和Burp Suite专业版的功能，
 * 前提是该使用不违反这些产品的许可条款。
 */

package burp.api.montoya.ui.editor;

import burp.api.montoya.core.ByteArray;
import burp.api.montoya.ui.Selection;

import java.awt.Component;
import java.util.Optional;

/**
 * 为扩展程序提供Burp Suite的HTTP文本编辑器实例，用于扩展程序自身的用户界面。
 */
public interface RawEditor extends Editor
{
    /**
     * 设置文本编辑器是否可编辑
     *
     * @param editable 布尔标志，控制文本编辑器是否可编辑
     */
    void setEditable(boolean editable);

    /**
     * @return 文本编辑器的当前内容
     */
    ByteArray getContents();

    /**
     * 以编程方式设置文本编辑器中的内容
     *
     * @param contents 要设置到文本编辑器中的内容
     */
    void setContents(ByteArray contents);

    /**
     * 更新编辑器下方搜索栏中显示的搜索表达式
     *
     * @param expression 搜索表达式
     */
    @Override
    void setSearchExpression(String expression);

    /**
     * @return 如果用户自上次编程设置内容后修改了编辑器内容，则返回true
     */
    @Override
    boolean isModified();

    /**
     * @return 当前编辑器中光标位置的索引
     */
    @Override
    int caretPosition();

    /**
     * 如果用户没有进行选择，则返回{@link Optional#empty()}
     *
     * @return 包含用户在编辑器中的当前选择的{@link Optional}对象
     */
    @Override
    Optional<Selection> selection();

    /**
     * @return 编辑器的UI组件，供扩展程序添加到自己的用户界面中
     */
    @Override
    Component uiComponent();
}
```
#### WebSocketMessageEditor
```java
/*
 * 版权所有 (c) 2023。PortSwigger Ltd. 保留所有权利。
 *
 * 此代码可用于扩展Burp Suite社区版和Burp Suite专业版的功能，
 * 前提是该使用不违反这些产品的许可条款。
 */

package burp.api.montoya.ui.editor;

import burp.api.montoya.core.ByteArray;
import burp.api.montoya.ui.Selection;

import java.awt.Component;
import java.util.Optional;

/**
 * 为扩展程序提供Burp Suite的WebSocket消息编辑器实例，用于扩展程序自身的用户界面。
 */
public interface WebSocketMessageEditor extends Editor
{
    /**
     * @return 消息编辑器的当前内容
     */
    ByteArray getContents();

    /**
     * 以编程方式设置消息编辑器中的内容
     *
     * @param contents 要设置到消息编辑器中的内容
     */
    void setContents(ByteArray contents);

    /**
     * 更新编辑器下方搜索栏中显示的搜索表达式
     *
     * @param expression 搜索表达式
     */
    @Override
    void setSearchExpression(String expression);

    /**
     * @return 如果用户自上次编程设置内容后修改了编辑器内容，则返回true
     */
    @Override
    boolean isModified();

    /**
     * @return 当前消息编辑器中光标位置的索引
     */
    @Override
    int caretPosition();

    /**
     * 如果用户没有进行选择，则返回{@link Optional#empty()}
     *
     * @return 包含用户在编辑器中的当前选择的{@link Optional}对象
     */
    @Override
    Optional<Selection> selection();

    /**
     * @return 编辑器的UI组件，供扩展程序添加到自己的用户界面中
     */
    @Override
    Component uiComponent();
}
```
#### extension
##### EditorCreationContext
```java
/*
 * 版权所有 (c) 2022-2023。PortSwigger Ltd. 保留所有权利。
 *
 * 此代码可用于扩展Burp Suite社区版和Burp Suite专业版的功能，
 * 前提是该使用不违反这些产品的许可条款。
 */

package burp.api.montoya.ui.editor.extension;

import burp.api.montoya.core.ToolSource;

/**
 * 该接口用于<code>ExtensionHttpRequestEditor</code>或<code>ExtensionHttpResponseEditor</code>
 * 获取当前显示消息的详细信息。创建Burp HTTP消息编辑器实例的扩展可以选择性地提供
 * <code>IMessageEditorController</code>的实现，当编辑器需要获取当前消息的更多信息时
 * (例如将其发送到其他Burp工具)会调用该实现。通过<code>IMessageEditorTabFactory</code>
 * 提供自定义编辑器标签页的扩展将为每个生成的标签页实例接收一个<code>IMessageEditorController</code>
 * 对象引用，标签页在需要获取当前消息的更多信息时可以调用该对象。
 */
public interface EditorCreationContext
{
    /**
     * 指示哪个Burp工具正在请求编辑器。
     *
     * @return 请求编辑器的工具来源
     */
    ToolSource toolSource();

    /**
     * 指示Burp工具请求的编辑器模式。
     * 例如Proxy期望只读编辑器，Repeater期望默认编辑器。
     *
     * @return 编辑器所需的模式
     */
    EditorMode editorMode();
}
```
##### EditorMode
```java
/*
 * 版权所有 (c) 2022-2023。PortSwigger Ltd. 保留所有权利。
 *
 * 此代码可用于扩展Burp Suite社区版和Burp Suite专业版的功能，
 * 前提是该使用不违反这些产品的许可条款。
 */

package burp.api.montoya.ui.editor.extension;

/**
 * 枚举类型，用于描述Burp Suite消息编辑器的不同模式。
 */
public enum EditorMode
{
    /**
     * 默认编辑模式，允许用户查看和修改消息内容
     */
    DEFAULT,
    
    /**
     * 只读模式，仅允许用户查看消息内容而不能修改
     */
    READ_ONLY
}
```
##### ExtensionProvidedEditor
```java
/*
 * 版权所有 (c) 2023。PortSwigger Ltd. 保留所有权利。
 *
 * 此代码可用于扩展Burp Suite社区版和Burp Suite专业版的功能，
 * 前提是该使用不违反这些产品的许可条款。
 */

package burp.api.montoya.ui.editor.extension;

import burp.api.montoya.http.message.HttpRequestResponse;
import burp.api.montoya.ui.Selection;

import java.awt.Component;

/**
 * 为不同类型的扩展提供的编辑器定义共享行为。
 */
public interface ExtensionProvidedEditor
{
    /**
     * 在编辑器组件中设置提供的{@link HttpRequestResponse}对象。
     *
     * @param requestResponse 要在编辑器中设置的请求和响应
     */
    void setRequestResponse(HttpRequestResponse requestResponse);

    /**
     * 检查HTTP消息编辑器是否对特定的{@link HttpRequestResponse}启用
     *
     * @param requestResponse 要检查的{@link HttpRequestResponse}
     *
     * @return 如果HTTP消息编辑器对提供的请求和响应启用则返回true
     */
    boolean isEnabledFor(HttpRequestResponse requestResponse);

    /**
     * @return 消息编辑器标签页标题中显示的标题
     */
    String caption();

    /**
     * @return 在消息编辑器标签页中渲染的组件
     */
    Component uiComponent();

    /**
     * 如果用户没有选择任何数据，该方法应返回{@code null}。
     *
     * @return 用户当前选择的数据
     */
    Selection selectedData();

    /**
     * @return 如果用户在编辑器中修改了当前消息则返回true
     */
    boolean isModified();
}
```
##### ExtensionProvidedHttpRequestEditor
```java
/*
 * 版权所有 (c) 2022-2023。PortSwigger Ltd. 保留所有权利。
 *
 * 此代码可用于扩展Burp Suite社区版和Burp Suite专业版的功能，
 * 前提是该使用不违反这些产品的许可条款。
 */

package burp.api.montoya.ui.editor.extension;

import burp.api.montoya.http.message.HttpRequestResponse;
import burp.api.montoya.http.message.requests.HttpRequest;
import burp.api.montoya.ui.Selection;

import java.awt.Component;

/**
 * 注册了{@link HttpRequestEditorProvider}的扩展必须返回此接口的实例。<br/>
 * Burp将使用该实例在其HTTP请求编辑器中创建自定义标签页。
 */
public interface ExtensionProvidedHttpRequestEditor extends ExtensionProvidedEditor
{
    /**
     * @return 从HTTP请求编辑器内容派生的{@link HttpRequest}实例
     */
    HttpRequest getRequest();

    /**
     * 在编辑器组件中设置提供的{@link HttpRequestResponse}对象。
     *
     * @param requestResponse 要在编辑器中设置的请求和响应
     */
    @Override
    void setRequestResponse(HttpRequestResponse requestResponse);

    /**
     * 检查HTTP消息编辑器是否对特定的{@link HttpRequestResponse}启用
     *
     * @param requestResponse 要检查的{@link HttpRequestResponse}
     *
     * @return 如果HTTP消息编辑器对提供的请求和响应启用则返回true
     */
    @Override
    boolean isEnabledFor(HttpRequestResponse requestResponse);

    /**
     * @return 消息编辑器标签页标题中显示的标题
     */
    @Override
    String caption();

    /**
     * @return 在消息编辑器标签页中渲染的组件
     */
    @Override
    Component uiComponent();

    /**
     * 如果用户没有选择任何数据，该方法应返回{@code null}。
     *
     * @return 用户当前选择的数据
     */
    @Override
    Selection selectedData();

    /**
     * @return 如果用户在编辑器中修改了当前消息则返回true
     */
    @Override
    boolean isModified();
}
```
##### ExtensionProvidedHttpResponseEditor
```java
/*
 * 版权所有 (c) 2022-2023。PortSwigger Ltd. 保留所有权利。
 *
 * 此代码可用于扩展Burp Suite社区版和Burp Suite专业版的功能，
 * 前提是该使用不违反这些产品的许可条款。
 */

package burp.api.montoya.ui.editor.extension;

import burp.api.montoya.http.message.HttpRequestResponse;
import burp.api.montoya.http.message.responses.HttpResponse;
import burp.api.montoya.ui.Selection;

import java.awt.Component;

/**
 * 注册了{@link HttpResponseEditorProvider}的扩展必须返回此接口的实例。<br/>
 * Burp将使用该实例在其HTTP响应编辑器中创建自定义标签页。
 */
public interface ExtensionProvidedHttpResponseEditor extends ExtensionProvidedEditor
{
    /**
     * @return 从HTTP响应编辑器内容派生的{@link HttpResponse}实例
     */
    HttpResponse getResponse();

    /**
     * 在编辑器组件中设置提供的{@link HttpRequestResponse}对象。
     *
     * @param requestResponse 要在编辑器中设置的请求和响应
     */
    @Override
    void setRequestResponse(HttpRequestResponse requestResponse);

    /**
     * 检查HTTP消息编辑器是否对特定的{@link HttpRequestResponse}启用
     *
     * @param requestResponse 要检查的{@link HttpRequestResponse}
     *
     * @return 如果HTTP消息编辑器对提供的请求和响应启用则返回true
     */
    @Override
    boolean isEnabledFor(HttpRequestResponse requestResponse);

    /**
     * @return 消息编辑器标签页标题中显示的标题
     */
    @Override
    String caption();

    /**
     * @return 在消息编辑器标签页中渲染的组件
     */
    @Override
    Component uiComponent();

    /**
     * 如果用户没有选择任何数据，该方法应返回{@code null}。
     *
     * @return 用户当前选择的数据
     */
    @Override
    Selection selectedData();

    /**
     * @return 如果用户在编辑器中修改了当前消息则返回true
     */
    @Override
    boolean isModified();
}
```
##### ExtensionProvidedWebSocketMessageEditor
```java
package burp.api.montoya.ui.editor.extension;

import burp.api.montoya.core.ByteArray;
import burp.api.montoya.ui.Selection;
import burp.api.montoya.ui.contextmenu.WebSocketMessage;

import java.awt.Component;

/**
 * 注册了{@link WebSocketMessageEditorProvider}的扩展必须返回此接口的实例。<br/>
 * Burp将使用该实例在其WebSocket消息编辑器中创建自定义标签页。
 */
public interface ExtensionProvidedWebSocketMessageEditor
{
    /**
     * @return 以{@link ByteArray}实例形式返回编辑器中当前设置的消息
     */
    ByteArray getMessage();
    
    /**
     * 在编辑器组件中设置提供的{@link WebSocketMessage}消息。
     *
     * @param message 要在编辑器中设置的消息
     */
    void setMessage(WebSocketMessage message);

    /**
     * 检查WebSocket编辑器是否对特定的{@link WebSocketMessage}消息启用
     *
     * @param message 要检查的{@link WebSocketMessage}消息
     *
     * @return 如果WebSocket消息编辑器对提供的消息启用则返回true
     */
    boolean isEnabledFor(WebSocketMessage message);

    /**
     * @return 消息编辑器标签页标题中显示的标题
     */
    String caption();

    /**
     * @return 在消息编辑器标签页中渲染的组件
     */
    Component uiComponent();

    /**
     * 如果用户没有选择任何数据，该方法应返回{@code null}。
     *
     * @return 用户当前选择的数据
     */
    Selection selectedData();

    /**
     * @return 如果用户在编辑器中修改了当前消息则返回true
     */
    boolean isModified();
}
```
##### HttpRequestEditorProvider
```java
/*
 * 版权所有 (c) 2022-2023。PortSwigger Ltd. 保留所有权利。
 *
 * 此代码可用于扩展Burp Suite社区版和Burp Suite专业版的功能，
 * 前提是该使用不违反这些产品的许可条款。
 */

package burp.api.montoya.ui.editor.extension;

/**
 * 扩展程序可以通过注册此接口的实例，在Burp的用户界面中提供自定义的HTTP请求编辑器。
 */
public interface HttpRequestEditorProvider
{
    /**
     * 当Burp需要从扩展程序获取新的HTTP请求编辑器时调用此方法。
     *
     * @param creationContext 包含需要请求编辑器的上下文详细信息
     *
     * @return 返回一个 {@link ExtensionProvidedHttpRequestEditor} 实例
     */
    ExtensionProvidedHttpRequestEditor provideHttpRequestEditor(EditorCreationContext creationContext);
}
```
##### HttpResponseEditorProvider
```java
/*
 * 版权所有 (c) 2022-2023。PortSwigger Ltd. 保留所有权利。
 *
 * 此代码可用于扩展Burp Suite社区版和Burp Suite专业版的功能，
 * 前提是该使用不违反这些产品的许可条款。
 */

package burp.api.montoya.ui.editor.extension;

/**
 * 扩展可以通过注册此接口的实例，在Burp用户界面中提供自定义的HTTP响应编辑器。
 */
public interface HttpResponseEditorProvider
{
    /**
     * 当Burp需要从扩展获取新的HTTP响应编辑器时调用此方法。
     *
     * @param creationContext 包含需要响应编辑器的上下文详细信息
     *
     * @return 返回一个 {@link ExtensionProvidedHttpResponseEditor} 实例
     */
    ExtensionProvidedHttpResponseEditor provideHttpResponseEditor(EditorCreationContext creationContext);
}
```
##### WebSocketMessageEditorProvider
```java
package burp.api.montoya.ui.editor.extension;

/**
 * 扩展可以通过注册此接口的实例，在Burp用户界面中提供自定义的WebSocket消息编辑器。
 */
public interface WebSocketMessageEditorProvider
{
    /**
     * 当Burp需要从扩展获取新的WebSocket消息编辑器时调用此方法。
     *
     * @param creationContext 包含需要消息编辑器的上下文详细信息
     *
     * @return 返回一个 {@link ExtensionProvidedWebSocketMessageEditor} 实例
     */
    ExtensionProvidedWebSocketMessageEditor provideMessageEditor(EditorCreationContext creationContext);
}
```
### hotkey
#### HotKeyContext
```java
/*
 * 版权所有 (c) 2022-2025。PortSwigger Ltd. 保留所有权利。
 *
 * 此代码可用于扩展Burp Suite社区版和Burp Suite专业版的功能，
 * 前提是该使用不违反这些产品的许可条款。
 */

package burp.api.montoya.ui.hotkey;

/**
 * 定义热键可触发的上下文环境枚举。
 * <p>
 * 目前仅支持HTTP消息编辑器上下文。
 * </p>
 */
public enum HotKeyContext
{
    /**
     * 表示热键可在HTTP消息编辑器上下文中触发。
     * <p>
     * 当用户焦点位于HTTP请求/响应编辑器时，
     * 注册在此上下文的热键才会被激活。
     * </p>
     */
    HTTP_MESSAGE_EDITOR
}
```
#### HotKeyEvent
```java
/*
 * 版权所有 (c) 2022-2025。PortSwigger Ltd. 保留所有权利。
 *
 * 此代码可用于扩展Burp Suite社区版和Burp Suite专业版的功能，
 * 前提是该使用不违反这些产品的许可条款。
 */

package burp.api.montoya.ui.hotkey;

import burp.api.montoya.core.ToolSource;
import burp.api.montoya.ui.contextmenu.ComponentEvent;
import burp.api.montoya.ui.contextmenu.InvocationSource;
import burp.api.montoya.ui.contextmenu.MessageEditorHttpRequestResponse;

import java.util.Optional;

/**
 * 提供由热键事件 {@link HotKeyHandler} 触发的上下文信息。
 * <p>
 * 该接口继承了组件事件、工具来源和调用来源接口，
 * 提供了热键触发时的各种上下文信息访问能力。
 * </p>
 */
public interface HotKeyEvent extends ComponentEvent, ToolSource, InvocationSource
{
    /**
     * 获取热键触发时当前选中的HTTP请求/响应详情。
     *
     * @return 包含当前选中请求响应及其元数据的 {@link Optional} 对象，
     *         如果无选中内容则返回空Optional
     */
    Optional<MessageEditorHttpRequestResponse> messageEditorRequestResponse();
}
```
#### HotKeyHandler
```java
/*
 * 版权所有 (c) 2022-2025。PortSwigger Ltd. 保留所有权利。
 *
 * 此代码可用于扩展Burp Suite社区版和Burp Suite专业版的功能，
 * 前提是该使用不违反这些产品的许可条款。
 */

package burp.api.montoya.ui.hotkey;

/**
 * 该接口允许扩展处理热键事件。
 * <p>
 * 实现此接口可以监听并响应Burp Suite中的热键操作。
 * </p>
 */
public interface HotKeyHandler
{
    /**
     * 当用户在界面中触发热键时由Burp Suite调用。
     *
     * @param event 热键事件对象，包含事件相关信息 {@link HotKeyEvent}
     */
    void handle(HotKeyEvent event);
}
```
### menu
#### BasicMenuItem
```java
package burp.api.montoya.ui.menu;

import static burp.api.montoya.internal.ObjectFactoryLocator.FACTORY;

/**
 * 表示一个基本的菜单项，继承自{@link MenuItem}接口。
 * <p>
 * 提供了菜单项的基本行为定义和操作方法，支持创建和修改菜单项实例。
 * </p>
 */
public interface BasicMenuItem extends MenuItem
{
    /**
     * 当点击该菜单项时执行的操作。
     */
    void action();

    /**
     * 创建一个带有新Runnable操作的菜单项副本。
     *
     * @param action 新的Runnable操作
     * @return 更新后的菜单项副本
     */
    BasicMenuItem withAction(Runnable action);

    /**
     * 创建一个带有新标题的菜单项副本。
     *
     * @param caption 新的菜单项标题
     * @return 更新后的菜单项副本
     */
    BasicMenuItem withCaption(String caption);

    /**
     * 创建一个新的基本菜单项实例。
     *
     * @param caption 菜单项的显示标题
     * @return 新的BasicMenuItem实例
     */
    static BasicMenuItem basicMenuItem(String caption)
    {
        return FACTORY.basicMenuItem(caption);
    }
}
```
#### Menu
```java
package burp.api.montoya.ui.menu;

import java.util.List;

import static burp.api.montoya.internal.ObjectFactoryLocator.FACTORY;

/**
 * 表示显示在{@link MenuBar}中的菜单。
 * <p>
 * 该接口定义了菜单的基本属性和操作方法，支持创建和修改菜单实例。
 * </p>
 */
public interface Menu
{
    /**
     * 获取菜单的显示标题。
     *
     * @return 菜单标题文本
     */
    String caption();

    /**
     * 获取菜单中包含的所有菜单项列表。
     *
     * @return 菜单项列表
     */
    List<MenuItem> menuItems();

    /**
     * 创建一个带有新标题的菜单副本。
     *
     * @param caption 新的菜单标题
     * @return 更新后的菜单副本
     */
    Menu withCaption(String caption);

    /**
     * 创建一个包含一个或多个菜单项的菜单副本。
     *
     * @param menuItems 要添加的菜单项数组
     * @return 更新后的菜单副本
     */
    Menu withMenuItems(MenuItem... menuItems);

    /**
     * 创建一个包含新菜单项列表的菜单副本。
     *
     * @param menuItems 新的菜单项列表
     * @return 更新后的菜单副本
     */
    Menu withMenuItems(List<MenuItem> menuItems);

    /**
     * 创建一个新的菜单实例。
     *
     * @param caption 菜单标题
     * @return 新的菜单实例
     */
    static Menu menu(String caption)
    {
        return FACTORY.menu(caption);
    }
}
```
#### MenuBar
```java
package burp.api.montoya.ui.menu;

import burp.api.montoya.core.Registration;
import javax.swing.JMenu;

/**
 * 表示主框架窗口的顶部菜单栏。
 * <p>
 * 提供注册自定义菜单的方法，支持两种菜单注册方式。
 * </p>
 */
public interface MenuBar
{
    /**
     * 注册一个Swing JMenu菜单到菜单栏。
     * <p>
     * 此方法适用于需要更精细控制菜单结构的情况。
     * </p>
     *
     * @param menu 要注册的Swing JMenu菜单对象
     * @return 菜单的注册句柄，可用于取消注册
     */
    Registration registerMenu(JMenu menu);

    /**
     * 注册一个Burp Menu菜单到菜单栏。
     * <p>
     * 此方法适用于添加简单的菜单项。
     * </p>
     *
     * @param menu 要注册的Burp Menu菜单对象
     * @return 菜单的注册句柄，可用于取消注册
     */
    Registration registerMenu(Menu menu);
}
```
#### MenuItem
```java
package burp.api.montoya.ui.menu;

import static burp.api.montoya.internal.ObjectFactoryLocator.FACTORY;

/**
 * 表示菜单中显示的菜单项。
 * <p>
 * 该接口定义了菜单项的基本属性和创建方法。
 * </p>
 */
public interface MenuItem
{
    /**
     * 获取菜单项的显示文本。
     *
     * @return 菜单项的标题文本
     */
    String caption();

    /**
     * 创建一个带有指定标题的基本菜单项实例。
     *
     * @param caption 菜单项的显示文本
     * @return 新创建的BasicMenuItem实例
     */
    static BasicMenuItem basicMenuItem(String caption)
    {
        return FACTORY.basicMenuItem(caption);
    }
}
```
### settings
#### SettingData
```java
package burp.api.montoya.ui.settings;

/**
 * 该接口用于从通过{@link SettingsPanelBuilder}创建的{@link SettingsPanel}中获取数据。
 * <p>
 * 提供了多种类型安全的方法来访问设置面板中的配置值。
 * </p>
 */
public interface SettingData
{
    /**
     * 获取字符串类型的设置值。
     *
     * @param name 设置项名称
     * @return 对应的字符串值，如果不存在则返回空字符串
     */
    String getString(String name);

    /**
     * 获取布尔类型的设置值。
     *
     * @param name 设置项名称
     * @return 对应的布尔值，如果不存在则返回false
     */
    boolean getBoolean(String name);

    /**
     * 获取整数类型的设置值。
     *
     * @param name 设置项名称
     * @return 对应的整数值，如果不存在则返回0
     */
    int getInteger(String name);
}
```
#### SettingsPanel
```java
package burp.api.montoya.ui.settings;

import javax.swing.JComponent;
import java.util.Set;

import static java.util.Collections.emptySet;

/**
 * 表示Burp设置对话框中显示的自定义设置面板。
 * <p>
 * 该接口定义了设置面板的基本行为和属性。
 * </p>
 */
public interface SettingsPanel
{
    /**
     * 获取要在设置对话框中显示的UI组件。
     *
     * @return 返回用于显示的Swing组件
     */
    JComponent uiComponent();

    /**
     * 获取用于设置搜索功能的关键词集合，帮助用户找到此面板。
     * <p>
     * 默认返回空集合，子类可重写此方法提供具体关键词。
     * </p>
     *
     * @return 关键词集合
     */
    default Set<String> keywords()
    {
        return emptySet();
    }
}
```
#### SettingsPanelBuilder
```java
package burp.api.montoya.ui.settings;

import java.util.Collection;

import static burp.api.montoya.internal.ObjectFactoryLocator.FACTORY;

/**
 * 用于构建{@link SettingsPanelWithData}实例的构建器接口。
 * <p>
 * 提供链式方法配置设置面板的各项属性。
 * </p>
 */
public interface SettingsPanelBuilder
{
    /**
     * 设置数据持久化类型。
     *
     * @param persistence 数据持久化类型枚举值
     * @return 当前构建器实例
     */
    SettingsPanelBuilder withPersistence(SettingsPanelPersistence persistence);

    /**
     * 设置显示在设置面板中的标题。
     * <p>
     * 如果未设置标题，将使用扩展名称作为默认标题。
     * </p>
     *
     * @param title 面板标题文本
     * @return 当前构建器实例
     */
    SettingsPanelBuilder withTitle(String title);

    /**
     * 设置显示在设置面板中的描述信息。
     *
     * @param description 描述文本
     * @return 当前构建器实例
     */
    SettingsPanelBuilder withDescription(String description);

    /**
     * 向设置面板添加单个配置项。
     *
     * @param entry 要添加的配置项
     * @return 当前构建器实例
     */
    SettingsPanelBuilder withSetting(SettingsPanelSetting entry);

    /**
     * 向设置面板添加多个配置项。
     *
     * @param entries 要添加的配置项数组
     * @return 当前构建器实例
     */
    SettingsPanelBuilder withSettings(SettingsPanelSetting... entries);

    /**
     * 设置用于帮助用户通过搜索找到该设置面板的关键词集合。
     *
     * @param keywords 关键词数组
     * @return 当前构建器实例
     */
    SettingsPanelBuilder withKeywords(String... keywords);

    /**
     * 设置用于帮助用户通过搜索找到该设置面板的关键词集合。
     *
     * @param keywords 关键词集合
     * @return 当前构建器实例
     */
    SettingsPanelBuilder withKeywords(Collection<String> keywords);

    /**
     * 构建并返回设置面板实例。
     *
     * @return 配置完成的设置面板实例
     */
    SettingsPanelWithData build();

    /**
     * 获取SettingsPanelBuilder的实例。
     *
     * @return SettingsPanelBuilder实例
     */
    static SettingsPanelBuilder settingsPanel()
    {
        return FACTORY.settingsPanel();
    }
}
```
#### SettingsPanelPersistence
```java
package burp.api.montoya.ui.settings;

/**
 * 定义{@link SettingsPanelWithData}的持久化行为模式。
 * <p>
 * 该枚举用于控制设置面板数据的存储方式和生命周期。
 * </p>
 */
public enum SettingsPanelPersistence
{
    /**
     * 设置值仅保存在内存中，Burp关闭时不会保存。
     * <p>
     * 适用于临时性、会话级的设置项。
     * </p>
     */
    NONE,

    /**
     * 设置保存在当前项目文件中。
     * <p>
     * 适用于项目特定的配置项，会随项目文件一起保存和加载。
     * </p>
     */
    PROJECT_SETTINGS,

    /**
     * 设置保存在用户数据中。
     * <p>
     * 适用于用户级的全局配置项，会跨项目持久化保存。
     * </p>
     */
    USER_SETTINGS
}
```
#### SettingsPanelSetting
```java
package burp.api.montoya.ui.settings;

import java.util.List;

import static burp.api.montoya.internal.ObjectFactoryLocator.FACTORY;

/**
 * 表示设置面板中的一个配置项。
 * <p>
 * 提供多种静态工厂方法用于创建不同类型的设置项。
 * </p>
 */
public interface SettingsPanelSetting
{
    /**
     * 创建一个仅接受整数值的文本输入框设置项。
     *
     * @param name 用于通过{@link SettingsPanelWithData}访问关联数据的名称
     * @return 设置项实例
     */
    static SettingsPanelSetting integerSetting(String name)
    {
        return FACTORY.integerSetting(name);
    }

    /**
     * 创建一个带有默认值的整型文本输入框设置项。
     * <p>
     * 如果存在同名持久化值，将优先使用持久化值而非默认值。
     * </p>
     *
     * @param name 用于通过{@link SettingsPanelWithData}访问关联数据的名称
     * @param defaultValue 初始默认值
     * @return 设置项实例
     */
    static SettingsPanelSetting integerSetting(String name, int defaultValue)
    {
        return FACTORY.integerSetting(name, defaultValue);
    }

    /**
     * 创建一个复选框设置项。
     *
     * @param name 用于通过{@link SettingsPanelWithData}访问关联数据的名称
     * @return 设置项实例
     */
    static SettingsPanelSetting booleanSetting(String name)
    {
        return FACTORY.booleanSetting(name);
    }

    /**
     * 创建一个带有默认选中状态的复选框设置项。
     * <p>
     * 如果存在同名持久化状态，将优先使用持久化状态而非默认状态。
     * </p>
     *
     * @param name 用于通过{@link SettingsPanelWithData}访问关联数据的名称
     * @param defaultValue 初始默认选中状态
     * @return 设置项实例
     */
    static SettingsPanelSetting booleanSetting(String name, boolean defaultValue)
    {
        return FACTORY.booleanSetting(name, defaultValue);
    }

    /**
     * 创建一个文本输入框设置项。
     *
     * @param name 用于通过{@link SettingsPanelWithData}访问关联数据的名称
     * @return 设置项实例
     */
    static SettingsPanelSetting stringSetting(String name)
    {
        return FACTORY.stringSetting(name);
    }

    /**
     * 创建一个带有默认文本值的输入框设置项。
     * <p>
     * 如果存在同名持久化值，将优先使用持久化值而非默认值。
     * </p>
     *
     * @param name 用于通过{@link SettingsPanelWithData}访问关联数据的名称
     * @param defaultValue 初始默认文本值
     * @return 设置项实例
     */
    static SettingsPanelSetting stringSetting(String name, String defaultValue)
    {
        return FACTORY.stringSetting(name, defaultValue);
    }

    /**
     * 创建一个下拉选择框设置项。
     *
     * @param name 用于通过{@link SettingsPanelWithData}访问关联数据的名称
     * @param values 下拉框中的可选值数组
     * @return 设置项实例
     */
    static SettingsPanelSetting listSetting(String name, String... values)
    {
        return FACTORY.listSetting(name, values);
    }

    /**
     * 创建一个带有默认选中值的下拉选择框设置项。
     * <p>
     * 如果存在同名持久化值，将优先使用持久化值而非默认值。
     * </p>
     *
     * @param name 用于通过{@link SettingsPanelWithData}访问关联数据的名称
     * @param values 下拉框中的可选值列表
     * @param defaultValue 初始默认选中值
     * @return 设置项实例
     */
    static SettingsPanelSetting listSetting(String name, List<String> values, String defaultValue)
    {
        return FACTORY.listSetting(name, values, defaultValue);
    }
}
```
#### SettingsPanelWithData
```java
package burp.api.montoya.ui.settings;

/**
 * 表示使用{@link SettingsPanelBuilder}构建的设置面板，
 * 该面板显示在Burp的设置对话框中并包含关联数据。
 * 
 * <p>该接口同时继承了{@link SettingsPanel}和{@link SettingData}的功能，
 * 既可作为设置面板使用，又能存储相关设置数据。</p>
 */
public interface SettingsPanelWithData extends SettingsPanel, SettingData
{
}
```
### swing
#### SwingUtils
```java
/*
 * 版权所有 (c) 2022-2023。PortSwigger Ltd. 保留所有权利。
 *
 * 此代码可用于扩展Burp Suite社区版和Burp Suite专业版的功能，
 * 前提是该使用不违反这些产品的许可条款。
 */

package burp.api.montoya.ui.swing;

import burp.api.montoya.core.HighlightColor;

import java.awt.Color;
import java.awt.Component;
import java.awt.Frame;
import java.awt.Window;

/**
 * 该接口提供了Swing相关的实用工具方法。
 */
public interface SwingUtils
{
    /**
     * 获取Burp套件的主框架窗口。
     *
     * @return Burp套件的主框架Frame对象
     */
    Frame suiteFrame();

    /**
     * 获取包含指定组件的顶层Window窗口。
     *
     * @param component 要查找的组件
     * @return 包含该组件的顶层Window对象
     */
    Window windowForComponent(Component component);

    /**
     * 将高亮颜色转换为Java Color对象。
     *
     * @param highlightColor 要转换的{@link HighlightColor}高亮颜色
     * @return 对应的Java Color对象
     */
    Color colorForHighLight(HighlightColor highlightColor);
}
```

## utillties
### Base64DecodingOptions
```java
/*
 * 版权所有 (c) 2022-2023。PortSwigger Ltd. 保留所有权利。
 *
 * 此代码可用于扩展Burp Suite社区版和Burp Suite专业版的功能，
 * 前提是该使用不违反这些产品的许可条款。
 */

package burp.api.montoya.utilities;

/**
 * 该枚举定义了Base64解码选项。
 */
public enum Base64DecodingOptions
{
    /**
     * 使用URL和文件名安全的Base64解码方案
     */
    URL
}
```
### Base64EncodingOptions
```java
/*
 * 版权所有 (c) 2022-2023。PortSwigger Ltd. 保留所有权利。
 *
 * 此代码可用于扩展Burp Suite社区版和Burp Suite专业版的功能，
 * 前提是该使用不违反这些产品的许可条款。
 */

package burp.api.montoya.utilities;

/**
 * 该枚举定义了Base64编码选项。
 */
public enum Base64EncodingOptions
{
    /**
     * 使用URL和文件名安全的Base64编码方案
     */
    URL,

    /**
     * 编码时不添加任何填充字符
     */
    NO_PADDING
}
```
### Base64Utils
```java
/*
 * 版权所有 (c) 2022-2023。PortSwigger Ltd. 保留所有权利。
 *
 * 此代码可用于扩展Burp Suite社区版和Burp Suite专业版的功能，
 * 前提是该使用不违反这些产品的许可条款。
 */

package burp.api.montoya.utilities;

import burp.api.montoya.core.ByteArray;

import java.util.Base64;

/**
 * 该接口提供了Base64编码和解码相关的功能方法。
 */
public interface Base64Utils
{
    /**
     * 使用{@link Base64}编码方案将指定字节数组中的所有字节编码到新分配的字节数组中。
     * 返回的字节数组长度等于编码结果的长度。
     *
     * @param data    要编码的字节数组
     * @param options 编码选项
     *
     * @return 包含编码结果的新分配字节数组
     */
    ByteArray encode(ByteArray data, Base64EncodingOptions... options);

    /**
     * 使用{@link Base64}编码方案将指定字符串中的所有字节编码到新分配的字节数组中。
     * 返回的字节数组长度等于编码结果的长度。
     *
     * @param data    要编码的字符串
     * @param options 编码选项
     *
     * @return 包含编码结果的新分配字节数组
     */
    ByteArray encode(String data, Base64EncodingOptions... options);

    /**
     * 使用{@link Base64}编码方案将指定字节数组中的所有字节编码为字符串。
     *
     * @param data    要编码的字节数组
     * @param options 编码选项
     *
     * @return 包含编码结果的字符串
     */
    String encodeToString(ByteArray data, Base64EncodingOptions... options);

    /**
     * 使用{@link Base64}编码方案将指定字符串中的所有字节编码为字符串。
     *
     * @param data    要编码的字符串
     * @param options 编码选项
     *
     * @return 包含编码结果的字符串
     */
    String encodeToString(String data, Base64EncodingOptions... options);

    /**
     * 使用{@link Base64}解码方案将指定字节数组中的所有字节解码到新分配的字节数组中。
     * 返回的字节数组长度等于解码结果的长度。
     *
     * @param data    要解码的字节数组
     * @param options 解码选项
     *
     * @return 包含解码结果的新分配字节数组
     */
    ByteArray decode(ByteArray data, Base64DecodingOptions... options);

    /**
     * 使用{@link Base64}解码方案将指定字符串中的所有字节解码到新分配的字节数组中。
     * 返回的字节数组长度等于解码结果的长度。
     *
     * @param data    要解码的字符串
     * @param options 解码选项
     *
     * @return 包含解码结果的新分配字节数组
     */
    ByteArray decode(String data, Base64DecodingOptions... options);
}
```
### ByteUtils
```java
/*
 * 版权所有 (c) 2022-2023。PortSwigger Ltd. 保留所有权利。
 *
 * 此代码可用于扩展Burp Suite社区版和Burp Suite专业版的功能，
 * 前提是该使用不违反这些产品的许可条款。
 */

package burp.api.montoya.utilities;

import java.util.regex.Pattern;

/**
 * 该接口提供了多种查询和操作字节数组的方法。
 */
public interface ByteUtils
{
    /**
     * 在数据中搜索指定模式的第一个出现位置。
     * 该方法对字节数据的操作方式类似于Java原生方法{@link String#indexOf(String)}对字符串数据的操作。
     *
     * @param data       待搜索的数据
     * @param searchTerm 要搜索的值
     *
     * @return 模式第一次出现的偏移量，如果未找到则返回-1
     */
    int indexOf(byte[] data, byte[] searchTerm);

    /**
     * 在数据中搜索指定模式的第一个出现位置。
     * 该方法对字节数据的操作方式类似于Java原生方法{@link String#indexOf(String)}对字符串数据的操作。
     *
     * @param data          待搜索的数据
     * @param searchTerm    要搜索的值
     * @param caseSensitive 是否区分大小写
     *
     * @return 模式第一次出现的偏移量，如果未找到则返回-1
     */
    int indexOf(byte[] data, byte[] searchTerm, boolean caseSensitive);

    /**
     * 在数据中搜索指定模式的第一个出现位置。
     * 该方法对字节数据的操作方式类似于Java原生方法{@link String#indexOf(String)}对字符串数据的操作。
     *
     * @param data          待搜索的数据
     * @param searchTerm    要搜索的值
     * @param caseSensitive 是否区分大小写
     * @param from          搜索起始偏移量
     * @param to            搜索结束偏移量
     *
     * @return 模式第一次出现的偏移量，如果未找到则返回-1
     */
    int indexOf(byte[] data, byte[] searchTerm, boolean caseSensitive, int from, int to);

    /**
     * 在数据中搜索指定正则模式的第一个出现位置。
     *
     * @param data    待搜索的数据
     * @param pattern 要匹配的正则模式
     *
     * @return 模式第一次出现的偏移量，如果未找到则返回-1
     */
    int indexOf(byte[] data, Pattern pattern);

    /**
     * 在数据中搜索指定正则模式的第一个出现位置。
     *
     * @param data    待搜索的数据
     * @param pattern 要匹配的正则模式
     * @param from    搜索起始偏移量
     * @param to      搜索结束偏移量
     *
     * @return 模式第一次出现的偏移量，如果未找到则返回-1
     */
    int indexOf(byte[] data, Pattern pattern, int from, int to);

    /**
     * 统计数据中指定模式的所有匹配次数。
     *
     * @param data       待搜索的数据
     * @param searchTerm 要搜索的值
     *
     * @return 模式匹配的总次数
     */
    int countMatches(byte[] data, byte[] searchTerm);

    /**
     * 统计数据中指定模式的所有匹配次数。
     *
     * @param data          待搜索的数据
     * @param searchTerm    要搜索的值
     * @param caseSensitive 是否区分大小写
     *
     * @return 模式匹配的总次数
     */
    int countMatches(byte[] data, byte[] searchTerm, boolean caseSensitive);

    /**
     * 统计数据中指定模式的所有匹配次数。
     *
     * @param data          待搜索的数据
     * @param searchTerm    要搜索的值
     * @param caseSensitive 是否区分大小写
     * @param from          搜索起始偏移量
     * @param to            搜索结束偏移量
     *
     * @return 指定范围内模式匹配的总次数
     */
    int countMatches(byte[] data, byte[] searchTerm, boolean caseSensitive, int from, int to);

    /**
     * 统计数据中指定正则模式的所有匹配次数。
     *
     * @param data    待搜索的数据
     * @param pattern 要匹配的正则模式
     *
     * @return 模式匹配的总次数
     */
    int countMatches(byte[] data, Pattern pattern);

    /**
     * 统计数据中指定正则模式的所有匹配次数。
     *
     * @param data    待搜索的数据
     * @param pattern 要匹配的正则模式
     * @param from    搜索起始偏移量
     * @param to      搜索结束偏移量
     *
     * @return 指定范围内模式匹配的总次数
     */
    int countMatches(byte[] data, Pattern pattern, int from, int to);

    /**
     * 将字节数组转换为字符串形式。
     * 该转换不反映任何特定字符集，字节0xYZ将始终转换为十六进制表示0x00YZ的字符。
     * 该方法与{@link ByteUtils#convertFromString(String)}执行相反的转换，
     * 使用这两个方法在字节数据和字符串之间转换可以保证数据的完整性（而使用特定字符集的转换可能无法保证）。
     *
     * @param bytes 要转换的数据
     *
     * @return 转换后的字符串
     */
    String convertToString(byte[] bytes);

    /**
     * 将字符串形式的数据转换为字节数组。
     * 该转换不反映任何特定字符集，十六进制表示0xWXYZ的字符将始终转换为字节0xYZ。
     * 该方法与{@link ByteUtils#convertToString(byte[])}执行相反的转换，
     * 使用这两个方法在字节数据和字符串之间转换可以保证数据的完整性（而使用特定字符集的转换可能无法保证）。
     *
     * @param string 要转换的字符串
     *
     * @return 转换后的字节数组
     */
    byte[] convertFromString(String string);
}
```
### CompressionType
```java
/*
 * 版权所有 (c) 2022-2023。PortSwigger Ltd. 保留所有权利。
 *
 * 此代码可用于扩展Burp Suite社区版和Burp Suite专业版的功能，
 * 前提是该使用不违反这些产品的许可条款。
 */

package burp.api.montoya.utilities;

/**
 * 该枚举定义了可用的压缩类型。
 */
public enum CompressionType
{
    /**
     * GZIP压缩格式
     */
    GZIP,
    
    /**
     * DEFLATE压缩格式
     */
    DEFLATE,
    
    /**
     * Google开发的Brotli压缩格式
     */
    BROTLI
}
```
### CompressionUtils
```java
/*
 * 版权所有 (c) 2022-2023。PortSwigger Ltd. 保留所有权利。
 *
 * 此代码可用于扩展Burp Suite社区版和Burp Suite专业版的功能，
 * 前提是该使用不违反这些产品的许可条款。
 */

package burp.api.montoya.utilities;

import burp.api.montoya.core.ByteArray;

/**
 * 该接口提供了数据压缩相关的功能操作。
 */
public interface CompressionUtils
{
    /**
     * 使用指定的压缩类型压缩数据。
     *
     * @param data 待压缩的数据
     * @param type 要使用的{@link CompressionType}压缩类型。目前仅支持GZIP
     *
     * @return 压缩后的数据
     */
    ByteArray compress(ByteArray data, CompressionType type);

    /**
     * 解压缩使用指定压缩类型压缩的数据。
     *
     * @param compressedData 待解压的压缩数据
     * @param type           压缩数据使用的{@link CompressionType}压缩类型
     *
     * @return 解压后的原始数据
     */
    ByteArray decompress(ByteArray compressedData, CompressionType type);
}
```
### CryptoUtils
```java
/*
 * 版权所有 (c) 2022-2023。PortSwigger Ltd. 保留所有权利。
 *
 * 此代码可用于扩展Burp Suite社区版和Burp Suite专业版的功能，
 * 前提是该使用不违反这些产品的许可条款。
 */

package burp.api.montoya.utilities;

import burp.api.montoya.core.ByteArray;

/**
 * 该接口提供了加密功能相关的操作方法。
 */
public interface CryptoUtils
{
    /**
     * 使用指定算法为输入数据生成消息摘要
     *
     * @param data      用于生成摘要的输入数据
     * @param algorithm 要使用的消息{@link DigestAlgorithm}摘要算法
     *
     * @return 生成的消息摘要
     */
    ByteArray generateDigest(ByteArray data, DigestAlgorithm algorithm);
}
```
### DigestAlgorithm
```java
/*
 * 版权所有 (c) 2022-2023。PortSwigger Ltd. 保留所有权利。
 *
 * 此代码可用于扩展Burp Suite社区版和Burp Suite专业版的功能，
 * 前提是该使用不违反这些产品的许可条款。
 */

package burp.api.montoya.utilities;

/**
 * 可用的消息摘要算法枚举。
 */
public enum DigestAlgorithm
{
    BLAKE2B_160("BLAKE2B-160"),
    BLAKE2B_256("BLAKE2B-256"),
    BLAKE2B_384("BLAKE2B-384"),
    BLAKE2B_512("BLAKE2B-512"),
    BLAKE2S_128("BLAKE2S-128"),
    BLAKE2S_160("BLAKE2S-160"),
    BLAKE2S_224("BLAKE2S-224"),
    BLAKE2S_256("BLAKE2S-256"),
    BLAKE3_256("BLAKE3-256"),
    DSTU7564_256("DSTU7564-256"),
    DSTU7564_384("DSTU7564-384"),
    DSTU7564_512("DSTU7564-512"),
    GOST3411("GOST3411"),
    GOST3411_2012_256("GOST3411-2012-256"),
    GOST3411_2012_512("GOST3411-2012-512"),
    HARAKA_256("HARAKA-256"),
    HARAKA_512("HARAKA-512"),
    KECCAK_224("KECCAK-224"),
    KECCAK_256("KECCAK-256"),
    KECCAK_288("KECCAK-288"),
    KECCAK_384("KECCAK-384"),
    KECCAK_512("KECCAK-512"),
    MD2("MD2"),
    MD4("MD4"),
    MD5("MD5"),
    PARALLEL_HASH_128_256("PARALLELHASH128-256"),
    PARALLEL_HASH_256_512("PARALLELHASH256-512"),
    RIPEMD_128("RIPEMD128"),
    RIPEMD_160("RIPEMD160"),
    RIPEMD_256("RIPEMD256"),
    RIPEMD_320("RIPEMD320"),
    SHA_1("SHA-1"),
    SHA_224("SHA-224"),
    SHA_256("SHA-256"),
    SHA_384("SHA-384"),
    SHA_512("SHA-512"),
    SHA_512_224("SHA-512/224"),
    SHA_512_256("SHA-512/256"),
    SHA3_224("SHA3-224"),
    SHA3_256("SHA3-256"),
    SHA3_384("SHA3-384"),
    SHA3_512("SHA3-512"),
    SHAKE_128_256("SHAKE128-256"),
    SHAKE_256_512("SHAKE256-512"),
    SKEIN_1024_1024("SKEIN-1024-1024"),
    SKEIN_1024_384("SKEIN-1024-384"),
    SKEIN_1024_512("SKEIN-1024-512"),
    SKEIN_256_128("SKEIN-256-128"),
    SKEIN_256_160("SKEIN-256-160"),
    SKEIN_256_224("SKEIN-256-224"),
    SKEIN_256_256("SKEIN-256-256"),
    SKEIN_512_128("SKEIN-512-128"),
    SKEIN_512_160("SKEIN-512-160"),
    SKEIN_512_224("SKEIN-512-224"),
    SKEIN_512_256("SKEIN-512-256"),
    SKEIN_512_384("SKEIN-512-384"),
    SKEIN_512_512("SKEIN-512-512"),
    SM3("SM3"),
    TIGER("TIGER"),
    TUPLEHASH_128_256("TUPLEHASH128-256"),
    TUPLEHASH_256_512("TUPLEHASH256-512"),
    WHIRLPOOL("WHIRLPOOL");

    public final String displayName;

    DigestAlgorithm(String displayName)
    {
        this.displayName = displayName;
    }
}
```
### HtmlEncoding
```java
/*
 * 版权所有 (c) 2022-2023。PortSwigger Ltd. 保留所有权利。
 *
 * 此代码可用于扩展Burp Suite社区版和Burp Suite专业版的功能，
 * 前提是该使用不违反这些产品的许可条款。
 */

package burp.api.montoya.utilities;

/**
 * 该枚举定义了HTML编码方式。
 */
public enum HtmlEncoding
{
    /**
     * 仅编码HTML特殊字符。
     */
    STANDARD,

    /**
     * 按照STANDARD方式编码HTML特殊字符，
     * 并将所有其他字符编码为十进制实体。
     */
    ALL_CHARACTERS,

    /**
     * 将所有字符编码为十进制实体。
     */
    ALL_CHARACTERS_DECIMAL,

    /**
     * 将所有字符编码为十六进制实体。
     */
    ALL_CHARACTERS_HEX
}
```
### HtmlUtils
```java
/*
 * 版权所有 (c) 2022-2023。PortSwigger Ltd. 保留所有权利。
 *
 * 本代码可用于扩展 Burp Suite 社区版和 Burp Suite 专业版的功能，
 * 前提是该使用不违反这些产品的许可条款。
 */

package burp.api.montoya.utilities;

/**
 * 该接口提供 HTML 编码和解码功能。
 */
public interface HtmlUtils
{
    /**
     * 使用 {@link HtmlEncoding#STANDARD} 编码对 HTML 文本进行编码
     *
     * @param html 要编码的 HTML 字符串
     * @return 编码后的字符串
     */
    String encode(String html);

    /**
     * 使用指定的编码方式对 HTML 文本进行编码
     *
     * @param html 要编码的 HTML 字符串
     * @param encoding 使用的 HTML 编码方式
     * @return 编码后的字符串
     */
    String encode(String html, HtmlEncoding encoding);

    /**
     * 解码已编码的 HTML 文本
     *
     * @param encodedHtml 要解码的已编码 HTML 字符串
     * @return 解码后的字符串
     */
    String decode(String encodedHtml);
}
```
### NumberUtils
```java
/*
 * 版权所有 (c) 2022-2023。PortSwigger Ltd. 保留所有权利。
 *
 * 本代码可用于扩展 Burp Suite 社区版和 Burp Suite 专业版的功能，
 * 前提是该使用不违反这些产品的许可条款。
 */

package burp.api.montoya.utilities;

import burp.api.montoya.core.ByteArray;

/**
 * 该接口提供数字字符串转换功能。
 */
public interface NumberUtils
{
    /**
     * 将二进制字符串转换为八进制表示
     *
     * @param binaryString 要转换的二进制字符串
     * @return 包含八进制表示的字符串
     */
    String convertBinaryToOctal(String binaryString);

    /**
     * 将字节数组转换为八进制表示
     *
     * @param byteArray 要转换的字节数组
     * @return 包含八进制表示的字符串
     */
    String convertBinaryToOctal(ByteArray byteArray);

    /**
     * 将二进制字符串转换为十进制表示
     *
     * @param binaryString 要转换的二进制字符串
     * @return 包含十进制表示的字符串
     */
    String convertBinaryToDecimal(String binaryString);

    /**
     * 将字节数组转换为十进制表示
     *
     * @param byteArray 要转换的字节数组
     * @return 包含十进制表示的字符串
     */
    String convertBinaryToDecimal(ByteArray byteArray);

    /**
     * 将二进制字符串转换为十六进制表示
     *
     * @param binaryString 要转换的二进制字符串
     * @return 包含十六进制表示的字符串
     */
    String convertBinaryToHex(String binaryString);

    /**
     * 将字节数组转换为十六进制表示
     *
     * @param byteArray 要转换的字节数组
     * @return 包含十六进制表示的字符串
     */
    String convertBinaryToHex(ByteArray byteArray);

    /**
     * 将八进制字符串转换为二进制表示
     *
     * @param octalString 要转换的八进制字符串
     * @return 包含二进制表示的字符串
     */
    String convertOctalToBinary(String octalString);

    /**
     * 将八进制字符串转换为十进制表示
     *
     * @param octalString 要转换的八进制字符串
     * @return 包含十进制表示的字符串
     */
    String convertOctalToDecimal(String octalString);

    /**
     * 将八进制字符串转换为十六进制表示
     *
     * @param octalString 要转换的八进制字符串
     * @return 包含十六进制表示的字符串
     */
    String convertOctalToHex(String octalString);

    /**
     * 将十进制字符串转换为二进制表示
     *
     * @param decimalString 要转换的十进制字符串
     * @return 包含二进制表示的字符串
     */
    String convertDecimalToBinary(String decimalString);

    /**
     * 将十进制字符串转换为八进制表示
     *
     * @param decimalString 要转换的十进制字符串
     * @return 包含八进制表示的字符串
     */
    String convertDecimalToOctal(String decimalString);

    /**
     * 将十进制字符串转换为十六进制表示
     *
     * @param decimalString 要转换的十进制字符串
     * @return 包含十六进制表示的字符串
     */
    String convertDecimalToHex(String decimalString);

    /**
     * 将十六进制字符串转换为二进制表示
     *
     * @param hexString 要转换的十六进制字符串
     * @return 包含二进制表示的字符串
     */
    String convertHexToBinary(String hexString);

    /**
     * 将十六进制字符串转换为八进制表示
     *
     * @param hexString 要转换的十六进制字符串
     * @return 包含八进制表示的字符串
     */
    String convertHexToOctal(String hexString);

    /**
     * 将十六进制字符串转换为十进制表示
     *
     * @param hexString 要转换的十六进制字符串
     * @return 包含十进制表示的字符串
     */
    String convertHexToDecimal(String hexString);

    /**
     * 将二进制字符串转换为指定基数的表示
     *
     * @param binaryString 要转换的二进制字符串
     * @param radix 要转换到的基数
     * @return 包含指定基数表示的字符串
     */
    String convertBinary(String binaryString, int radix);

    /**
     * 将八进制字符串转换为指定基数的表示
     *
     * @param octalString 要转换的八进制字符串
     * @param radix 要转换到的基数
     * @return 包含指定基数表示的字符串
     */
    String convertOctal(String octalString, int radix);

    /**
     * 将十进制字符串转换为指定基数的表示
     *
     * @param decimalString 要转换的十进制字符串
     * @param radix 要转换到的基数
     * @return 包含指定基数表示的字符串
     */
    String convertDecimal(String decimalString, int radix);

    /**
     * 将十六进制字符串转换为指定基数的表示
     *
     * @param hexString 要转换的十六进制字符串
     * @param radix 要转换到的基数
     * @return 包含指定基数表示的字符串
     */
    String convertHex(String hexString, int radix);
}
```
### RandomUtils
```java
/*
 * 版权所有 (c) 2022-2023。PortSwigger Ltd. 保留所有权利。
 *
 * 本代码可用于扩展 Burp Suite 社区版和 Burp Suite 专业版的功能，
 * 前提是该使用不违反这些产品的许可条款。
 */

package burp.api.montoya.utilities;

import java.util.Arrays;
import java.util.stream.Collectors;

/**
 * 随机字符串生成工具接口
 */
public interface RandomUtils
{
    /**
     * 使用字母数字字符生成随机字符串
     *
     * @param length 生成的随机字符串长度
     * @return 随机生成的字符串
     */
    String randomString(int length);

    /**
     * 使用指定字符集生成随机字符串
     *
     * @param length 生成的随机字符串长度
     * @param chars 用于生成字符串的字符集
     * @return 随机生成的字符串
     */
    String randomString(int length, String chars);

    /**
     * 使用指定的字符集枚举生成随机字符串
     *
     * @param length 生成的随机字符串长度
     * @param characterSets 用于生成字符串的字符集枚举数组
     * @return 随机生成的字符串
     */
    String randomString(int length, CharacterSet... characterSets);

    /**
     * 使用指定字符集生成指定长度范围的随机字符串
     *
     * @param minLength 生成字符串的最小长度(包含)
     * @param maxLength 生成字符串的最大长度(包含)
     * @param chars 用于生成字符串的字符集
     * @return 随机生成的字符串
     */
    String randomString(int minLength, int maxLength, String chars);

    /**
     * 使用指定的字符集枚举生成指定长度范围的随机字符串
     *
     * @param minLength 生成字符串的最小长度(包含)
     * @param maxLength 生成字符串的最大长度(包含)
     * @param characterSets 用于生成字符串的字符集枚举数组
     * @return 随机生成的字符串
     */
    String randomString(int minLength, int maxLength, CharacterSet... characterSets);

    /**
     * 预定义的字符集枚举
     */
    enum CharacterSet
    {
        ASCII_LOWERCASE("abcdefghijklmnopqrstvwxyz"),  // 小写字母字符集
        ASCII_UPPERCASE("ABCDEFGHIJKLMNOPQRSTVWXYZ"),  // 大写字母字符集
        ASCII_LETTERS(ASCII_LOWERCASE, ASCII_UPPERCASE),  // 所有字母字符集(大小写)
        DIGITS("0123456789"),  // 数字字符集
        PUNCTUATION("!\"#$%&'()*+,-./:;=<>?@[\\]^_`{|}~."),  // 标点符号字符集
        WHITESPACE(" \t\n\u000b\r\f"),  // 空白字符集
        PRINTABLE(DIGITS, ASCII_LETTERS, PUNCTUATION, WHITESPACE);  // 可打印字符集

        public final String characters;  // 字符集包含的实际字符

        CharacterSet(String characters)  // 通过字符串构造字符集
        {
            this.characters = characters;
        }

        CharacterSet(CharacterSet... charsList)  // 通过组合其他字符集构造新字符集
        {
            characters = Arrays.stream(charsList).map(charSet -> charSet.characters).collect(Collectors.joining());
        }
    }
}
```
### StringUtils
```java
/*
 * 版权所有 (c) 2022-2023。PortSwigger Ltd. 保留所有权利。
 *
 * 本代码可用于扩展 Burp Suite 社区版和 Burp Suite 专业版的功能，
 * 前提是该使用不违反这些产品的许可条款。
 */

package burp.api.montoya.utilities;

/**
 * 该接口提供字符串操作功能。
 */
public interface StringUtils
{
    /**
     * 将字符串转换为其ASCII字符的十六进制表示。
     * 每个字符将被转换为两位十六进制值。
     *
     * @param data 要转换的ASCII数据
     * @return 十六进制值组成的字符串
     */
    String convertAsciiToHexString(String data);

    /**
     * 将十六进制字符串转换为ASCII字符字符串。
     * 每对十六进制数字将被转换为单个ASCII字符。
     *
     * @param data 要转换的十六进制字符串
     * @return ASCII字符组成的字符串
     */
    String convertHexStringToAscii(String data);
}
```
### URLEncoding
```java
/*
 * 版权所有 (c) 2022-2025。PortSwigger Ltd. 保留所有权利。
 *
 * 本代码可用于扩展 Burp Suite 社区版和 Burp Suite 专业版的功能，
 * 前提是该使用不违反这些产品的许可条款。
 */

package burp.api.montoya.utilities;

/**
 * URL 编码方式枚举类型。
 */
public enum URLEncoding
{
    /**
     * 使用 {@link java.net.URLEncoder} 进行编码（Java 默认方式）
     */
    JAVA_DEFAULT,

    /**
     * 仅对关键字符进行编码
     */
    KEY_CHARACTERS,

    /**
     * 对所有字符进行编码
     */
    ALL_CHARACTERS,

    /**
     * 将所有字符编码为 Unicode 格式
     */
    ALL_CHARACTERS_UNICODE
}
```
### URLUtils
```java
/*
 * 版权所有 (c) 2022-2023。PortSwigger Ltd. 保留所有权利。
 *
 * 此代码可用于扩展Burp Suite社区版和Burp Suite专业版的功能，
 * 前提是该使用不违反这些产品的许可条款。
 */

package burp.api.montoya.utilities;

import burp.api.montoya.core.ByteArray;

/**
 * 该接口提供URL编码和解码功能。
 */
public interface URLUtils
{
    /**
     * 此方法等效于使用 {@link URLEncoding#JAVA_DEFAULT} 调用 {@link #encode(String, URLEncoding)}。
     *
     * @param string 要进行URL编码的{@code String}
     *
     * @return URL编码后的{@code String}
     *
     * @see java.net.URLEncoder#encode(String, String)
     */
    String encode(String string);

    /**
     * @param string 要进行URL编码的{@code String}
     * @param encoding 使用的{@link URLEncoding}编码方式
     *
     * @return URL编码后的{@code String}
     */
    String encode(String string, URLEncoding encoding);

    /**
     * @param string 要进行URL解码的{@code String}
     *
     * @return URL解码后的{@code String}
     *
     * @see java.net.URLDecoder#decode(String, String)
     */
    String decode(String string);

    /**
     * @param byteArray 要进行URL编码的{@link ByteArray}
     *
     * @return URL编码后的{@link ByteArray}
     *
     * @see java.net.URLEncoder#encode(String, String)
     */
    ByteArray encode(ByteArray byteArray);

    /**
     * @param byteArray 要进行URL解码的{@link ByteArray}
     *
     * @return URL解码后的{@link ByteArray}
     *
     * @see java.net.URLDecoder#decode(String, String)
     */
    ByteArray decode(ByteArray byteArray);
}
```
### Utilities
```java
/*
 * 版权所有 (c) 2022-2023。PortSwigger Ltd. 保留所有权利。
 *
 * 此代码可用于扩展Burp Suite社区版和Burp Suite专业版的功能，
 * 前提是该使用不违反这些产品的许可条款。
 */

package burp.api.montoya.utilities;

import burp.api.montoya.utilities.json.JsonUtils;
import burp.api.montoya.utilities.shell.ShellUtils;

/**
 * 该接口提供访问其他具有各种数据转换、查询和杂项功能的接口。
 */
public interface Utilities
{
    /**
     * @return {@link burp.api.montoya.utilities.Base64Utils} 的实例
     */
    Base64Utils base64Utils();

    /**
     * @return {@link burp.api.montoya.utilities.ByteUtils} 的实例
     */
    ByteUtils byteUtils();

    /**
     * @return {@link burp.api.montoya.utilities.CompressionUtils} 的实例
     */
    CompressionUtils compressionUtils();

    /**
     * @return {@link burp.api.montoya.utilities.CryptoUtils} 的实例
     */
    CryptoUtils cryptoUtils();

    /**
     * @return {@link burp.api.montoya.utilities.HtmlUtils} 的实例
     */
    HtmlUtils htmlUtils();

    /**
     * @return {@link burp.api.montoya.utilities.NumberUtils} 的实例
     */
    NumberUtils numberUtils();

    /**
     * @return {@link burp.api.montoya.utilities.RandomUtils} 的实例
     */
    RandomUtils randomUtils();

    /**
     * @return {@link burp.api.montoya.utilities.StringUtils} 的实例
     */
    StringUtils stringUtils();

    /**
     * @return {@link burp.api.montoya.utilities.URLUtils} 的实例
     */
    URLUtils urlUtils();

    /**
     * @return {@link JsonUtils} 的实例
     */
    JsonUtils jsonUtils();

    /**
     * @return {@link ShellUtils} 的实例
     */
    ShellUtils shellUtils();
}
```
### json
#### JsonArrayNode
```java
/*
 * 版权所有 (c) 2022-2024。PortSwigger Ltd. 保留所有权利。
 *
 * 此代码可用于扩展Burp Suite社区版和Burp Suite专业版的功能，
 * 前提是该使用不违反这些产品的许可条款。
 */

package burp.api.montoya.utilities.json;

import java.util.List;

import static burp.api.montoya.internal.ObjectFactoryLocator.FACTORY;

/**
 * 此接口用于定义JSON数组节点。
 *
 * <p><em>注意：底层列表可直接访问。对该列表的修改会直接反映在节点上。如需避免此行为，可操作列表的副本。</em></p>
 */
public interface JsonArrayNode extends JsonNode
{
    @Override
    List<JsonNode> getValue();

    /**
     * 获取此{@link JsonNode}的值列表。
     *
     * @return 此JsonNode的值列表
     */
    List<JsonNode> asList();

    /**
     * 向此{@link JsonArrayNode}添加一个{@link JsonNode}。
     *
     * @param value 要添加的节点
     */
    void add(JsonNode value);

    /**
     * 向此{@link JsonArrayNode}添加一个字符串。
     *
     * @param value 要添加的字符串
     * @throws NullPointerException 如果值为null
     */
    void addString(String value);

    /**
     * 向此{@link JsonArrayNode}添加一个布尔值。
     *
     * @param value 要添加的布尔值
     */
    void addBoolean(boolean value);

    /**
     * 向此{@link JsonArrayNode}添加一个long数值。
     *
     * @param value 要添加的long值
     */
    void addNumber(long value);

    /**
     * 向此{@link JsonArrayNode}添加一个double数值。
     *
     * @param value 要添加的double值
     */
    void addNumber(double value);

    /**
     * 向此{@link JsonArrayNode}添加一个Number数值。
     *
     * @param value 要添加的Number值
     */
    void addNumber(Number value);

    /**
     * 尝试获取指定索引处的JsonNode。
     *
     * @param index 要获取的索引位置
     * @return 指定索引处的{@link JsonNode}
     */
    JsonNode get(int index);

    /**
     * 尝试获取指定索引处的字符串。
     *
     * @param index 要获取的索引位置
     * @return 指定索引处的字符串，如果不是字符串类型则返回null
     */
    String getString(int index);

    /**
     * 尝试获取指定索引处的布尔值。
     *
     * @param index 要获取的索引位置
     * @return 指定索引处的布尔值，如果不是布尔类型则返回null
     */
    Boolean getBoolean(int index);

    /**
     * 尝试获取指定索引处的数值并作为long返回。
     *
     * @param index 要获取的索引位置
     * @return 指定索引处的long值，如果不是数值类型则返回null
     */
    Long getLong(int index);

    /**
     * 尝试获取指定索引处的数值并作为double返回。
     *
     * @param index 要获取的索引位置
     * @return 指定索引处的double值，如果不是数值类型则返回null
     */
    Double getDouble(int index);

    /**
     * 尝试获取指定索引处的数值。
     *
     * @param index 要获取的索引位置
     * @return 指定索引处的Number值，如果不是数值类型则返回null
     */
    Number getNumber(int index);

    /**
     * 移除指定索引处的JsonNode。
     *
     * @param index 要移除的节点索引位置
     */
    void remove(int index);

    /**
     * 创建一个新的空{@link JsonArrayNode}实例。
     *
     * @return 新的{@link JsonArrayNode}实例
     */
    static JsonArrayNode jsonArrayNode()
    {
        return FACTORY.jsonArrayNode();
    }

    /**
     * 从提供的{@link JsonNode}列表创建新的{@link JsonArrayNode}实例。
     *
     * @param value {@link JsonNode}列表
     * @return 新的{@link JsonNode}实例
     */
    static JsonArrayNode jsonArrayNode(List<? extends JsonNode> value)
    {
        return FACTORY.jsonArrayNode(value);
    }

    /**
     * 从提供的{@link JsonNode}实例数组创建新的{@link JsonArrayNode}实例。
     *
     * @param values {@link JsonNode}实例数组
     * @return 新的{@link JsonNode}实例
     */
    static JsonArrayNode jsonArrayNode(JsonNode... values)
    {
        return FACTORY.jsonArrayNode(values);
    }
}
```
#### JsonBooleanNode
```java
/*
 * 版权所有 (c) 2022-2024。PortSwigger Ltd. 保留所有权利。
 *
 * 此代码可用于扩展Burp Suite社区版和Burp Suite专业版的功能，
 * 前提是该使用不违反这些产品的许可条款。
 */

package burp.api.montoya.utilities.json;

import static burp.api.montoya.internal.ObjectFactoryLocator.FACTORY;

/**
 * 此接口用于定义JSON布尔值节点。
 */
public interface JsonBooleanNode extends JsonNode
{
    @Override
    Boolean getValue();

    /**
     * 从提供的布尔值创建新的{@link JsonBooleanNode}实例。
     *
     * @param value 布尔类型的值
     * @return 新的{@link JsonBooleanNode}实例
     */
    static JsonBooleanNode jsonBooleanNode(boolean value)
    {
        return FACTORY.jsonBooleanNode(value);
    }
}
```
#### JsonException
```java
/*
 * 版权所有 (c) 2022-2024。PortSwigger Ltd. 保留所有权利。
 *
 * 此代码可用于扩展Burp Suite社区版和Burp Suite专业版的功能，
 * 前提是该使用不违反这些产品的许可条款。
 */

package burp.api.montoya.utilities.json;

/**
 * 此类表示在尝试执行不成功操作时抛出的异常。
 */
public class JsonException extends RuntimeException
{
    /**
     * 使用指定的错误消息构造JsonException
     *
     * @param message 异常的错误描述信息
     */
    public JsonException(String message)
    {
        super(message);
    }
}
```
#### JsonNode
```java
/*
 * 版权所有 (c) 2022-2024。PortSwigger Ltd. 保留所有权利。
 *
 * 此代码可用于扩展Burp Suite社区版和Burp Suite专业版的功能，
 * 前提是该使用不违反这些产品的许可条款。
 */

package burp.api.montoya.utilities.json;

import static burp.api.montoya.internal.ObjectFactoryLocator.FACTORY;

/**
 * <p>此接口用于表示JSON节点，作为所有其他JsonNode类型的基础接口（参见子接口）。</p>
 * <p>您可以使用{@link JsonNode#jsonNode(String)}从原始JSON字符串创建JsonNode，该方法会尝试将JSON解析为特定的JsonNode类型。</p>
 * <p>要将创建的JsonNode作为JsonArray或JsonObject获取，可分别调用{@link #asArray()}或{@link #asObject()}。这为特定的JSON类型提供了额外的实用功能。</p>
 *
 * <p><em>注意：如果要解析JsonObject，可以使用{@link JsonObjectNode}及其实用方法。在JsonNode上调用{@link #asObject}。</em></p>
 * <p><em>注意：尝试将JsonNode作为错误类型获取会抛出{@link IllegalStateException}。</em></p>
 *
 * <pre>
 *     JsonNode jsonNode = JsonNode.create("[]");       // 解析字符串，底层类型将是{@link JsonArrayNode}
 *     JsonObjectNode objectNode = jsonNode.asObject(); // 抛出IllegalStateException，因为JSON字符串被解析为{@link JsonArrayNode}
 *     JsonArrayNode arrayNode = jsonNode.asArray();    // 成功
 * </pre>
 *
 * <p>
 *     每种特定的JsonNode类型都有对应的工厂方法：
 * </p>
 *
 * <pre>
 *     // 创建值为"foo"的JsonStringNode
 *     JsonStringNode stringNode = JsonStringNode.jsonStringNode("foo");
 *
 *     // 创建值为2.5的JsonNumberNode
 *     JsonNumberNode numberNode = JsonNumberNode.jsonNumberNode(2.5);
 *
 *     // 创建包含上述两个节点的JsonArrayNode
 *     JsonArrayNode arrayNode = JsonArrayNode.jsonArrayNode(
 *         stringNode,
 *         numberNode
 *     );
 * </pre>
 *
 * <p>
 *     任何JsonNode都可以返回其JSON字符串表示：
 * </p>
 *
 * <pre>
 *     JsonArrayNode arrayNode = JsonArrayNode.jsonArrayNode(
 *            stringNode,
 *            numberNode
 *     );
 *
 *     String arrayNodeAsJson = arrayNode.toJsonString();
 *
 *     System.out.println(arrayNodeAsJson);
 * </pre>
 *
 * <p>输出：</p>
 *
 * <pre>
 *     [
 *       "foo",
 *       2.5
 *     ]
 * </pre>
 */
public interface JsonNode
{
    /**
     * 获取此{@link JsonNode}的值。
     *
     * @return 此JsonNode的值。
     */
    Object getValue();

    /**
     * 将此{@link JsonNode}作为字符串表示返回。
     *
     * @return JsonNode的JSON字符串格式。
     */
    String toJsonString();

    /**
     * 检查此{@link JsonNode}是否为数组。
     *
     * @return 如果此JsonNode表示JSON数组则返回true。
     */
    boolean isArray();

    /**
     * 检查此{@link JsonNode}是否为对象。
     *
     * @return 如果此JsonNode表示JSON对象则返回true。
     */
    boolean isObject();

    /**
     * 检查此{@link JsonNode}是否为字符串。
     *
     * @return 如果此JsonNode表示JSON字符串则返回true。
     */
    boolean isString();

    /**
     * 检查此{@link JsonNode}是否为数字。
     *
     * @return 如果此JsonNode表示JSON数字则返回true。
     */
    boolean isNumber();

    /**
     * 检查此{@link JsonNode}是否为布尔值。
     *
     * @return 如果此JsonNode表示JSON布尔值则返回true。
     */
    boolean isBoolean();

    /**
     * 检查此{@link JsonNode}是否为null。
     *
     * @return 如果此JsonNode表示null值则返回true。
     */
    boolean isNull();

    /**
     * 尝试将此{@link JsonNode}作为布尔值返回。
     *
     * @throws IllegalStateException 如果此JsonNode不是布尔类型。
     */
    Boolean asBoolean();

    /**
     * 尝试将此{@link JsonNode}作为字符串返回。
     *
     * @throws IllegalStateException 如果此JsonNode不是字符串类型。
     */
    String asString();

    /**
     * 尝试将此{@link JsonNode}作为数字返回。
     *
     * @throws IllegalStateException 如果此JsonNode不是数字类型。
     */
    Number asNumber();

    /**
     * 尝试将此{@link JsonNode}作为long返回。
     *
     * @throws IllegalStateException 如果此JsonNode不是数字类型。
     */
    Long asLong();

    /**
     * 尝试将此{@link JsonNode}作为double返回。
     *
     * @throws IllegalStateException 如果此JsonNode不是数字类型。
     */
    Double asDouble();

    /**
     * 尝试将此{@link JsonNode}作为节点列表返回。
     *
     * @throws IllegalStateException 如果此JsonNode不是数组类型。
     */
    JsonArrayNode asArray();

    /**
     * 尝试将此{@link JsonNode}作为对象返回。
     *
     * @throws IllegalStateException 如果此JsonNode不是对象类型。
     */
    JsonObjectNode asObject();

    /**
     * 从提供的json字符串创建新的{@link JsonNode}实例。
     *
     * @param json JSON字符串，可以使用单引号代替双引号。
     *
     * @return 新的{@link JsonNode}实例。
     * @throws JsonParseException 如果字符串不是有效的JSON。
     */
    static JsonNode jsonNode(String json)
    {
        return FACTORY.jsonNode(json);
    }
}
```
#### JsonNullNode
```java
/*
 * 版权所有 (c) 2022-2024。PortSwigger Ltd. 保留所有权利。
 *
 * 此代码可用于扩展Burp Suite社区版和Burp Suite专业版的功能，
 * 前提是该使用不违反这些产品的许可条款。
 */

package burp.api.montoya.utilities.json;

import static burp.api.montoya.internal.ObjectFactoryLocator.FACTORY;

/**
 * 此接口用于定义JSON空值(null)节点。
 */
public interface JsonNullNode extends JsonNode
{
    @Override
    Object getValue();

    /**
     * 创建新的{@link JsonNullNode}实例。
     *
     * @return 新的{@link JsonNullNode}实例。
     */
    static JsonNullNode jsonNullNode()
    {
        return FACTORY.jsonNullNode();
    }
}
```
#### JsonNumberNode
```java
/*
 * 版权所有 (c) 2022-2024。PortSwigger Ltd. 保留所有权利。
 *
 * 此代码可用于扩展Burp Suite社区版和Burp Suite专业版的功能，
 * 前提是该使用不违反这些产品的许可条款。
 */

package burp.api.montoya.utilities.json;

import static burp.api.montoya.internal.ObjectFactoryLocator.FACTORY;

/**
 * 此接口用于定义JSON数字节点。
 */
public interface JsonNumberNode extends JsonNode
{
    @Override
    Number getValue();

    /**
     * 从提供的long值创建新的{@link JsonNumberNode}实例。
     *
     * @param value long类型的值。
     *
     * @return 新的{@link JsonNumberNode}实例。
     */
    static JsonNumberNode jsonNumberNode(long value)
    {
        return FACTORY.jsonNumberNode(value);
    }

    /**
     * 从提供的double值创建新的{@link JsonNumberNode}实例。
     *
     * @param value double类型的值。
     *
     * @return 新的{@link JsonNumberNode}实例。
     */
    static JsonNumberNode jsonNumberNode(double value)
    {
        return FACTORY.jsonNumberNode(value);
    }

    /**
     * 从提供的Number对象创建新的{@link JsonNumberNode}实例。
     *
     * @param value Number类型的值。
     *
     * @return 新的{@link JsonNumberNode}实例。
     */
    static JsonNumberNode jsonNumberNode(Number value)
    {
        return FACTORY.jsonNumberNode(value);
    }
}
```
#### JsonObjectNode
```java
/*
 * 版权所有 (c) 2022-2024。PortSwigger Ltd. 保留所有权利。
 *
 * 此代码可用于扩展 Burp Suite Community Edition 和 Burp Suite Professional 的功能，
 * 前提是该使用不违反这些产品的许可条款。
 */

package burp.api.montoya.utilities.json;

import java.util.Map;

import static burp.api.montoya.internal.ObjectFactoryLocator.FACTORY;

/**
 * 该接口用于定义JSON对象节点
 *
 * <p><em>注意：可以访问底层Map，对此Map的修改会直接反映在节点上。如需避免此行为，可操作Map的副本。</em></p>
 */
public interface JsonObjectNode extends JsonNode
{
    @Override
    Map<String, JsonNode> getValue();

    /**
     * 获取此JSON节点的值
     *
     * @return 包含键值对的Map
     */
    Map<String, JsonNode> asMap();

    /**
     * 向此JSON对象节点中添加一个JSON节点
     *
     * @param key 键名
     * @param value 对应的JSON节点值
     */
    void put(String key, JsonNode value);

    /**
     * 向此JSON对象节点中添加字符串值
     *
     * @param key 键名
     * @param value 字符串值
     * @throws NullPointerException 当值为null时抛出
     */
    void putString(String key, String value);

    /**
     * 向此JSON对象节点中添加布尔值
     *
     * @param key 键名
     * @param value 布尔值
     */
    void putBoolean(String key, boolean value);

    /**
     * 向此JSON对象节点中添加长整型数值
     *
     * @param key 键名
     * @param value 长整型数值
     */
    void putNumber(String key, long value);

    /**
     * 向此JSON对象节点中添加双精度浮点数值
     *
     * @param key 键名
     * @param value 双精度浮点数值
     */
    void putNumber(String key, double value);

    /**
     * 向此JSON对象节点中添加数值
     *
     * @param key 键名
     * @param value 数值
     */
    void putNumber(String key, Number value);

    /**
     * 尝试获取指定键对应的JSON节点
     *
     * @param key 键名
     * @return 对应的JSON节点，若不存在或类型不匹配则返回null
     */
    JsonNode get(String key);

    /**
     * 尝试获取指定键对应的字符串值
     *
     * @param key 键名
     * @return 字符串值，若不存在或类型不匹配则返回null
     */
    String getString(String key);

    /**
     * 尝试获取指定键对应的布尔值
     *
     * @param key 键名
     * @return 布尔值，若不存在或类型不匹配则返回null
     */
    Boolean getBoolean(String key);

    /**
     * 尝试获取指定键对应的长整型数值
     *
     * @param key 键名
     * @return 长整型数值，若不存在或类型不匹配则返回null
     */
    Long getLong(String key);

    /**
     * 尝试获取指定键对应的双精度浮点数值
     *
     * @param key 键名
     * @return 双精度浮点数值，若不存在或类型不匹配则返回null
     */
    Double getDouble(String key);

    /**
     * 尝试获取指定键对应的数值
     *
     * @param key 键名
     * @return 数值，若不存在或类型不匹配则返回null
     */
    Number getNumber(String key);

    /**
     * 从对象中移除指定键及其对应的JSON节点
     *
     * @param key 要移除的键名
     */
    void remove(String key);

    /**
     * 检查对象是否包含指定键
     *
     * @param key 要检查的键名
     * @return 如果包含则返回true
     */
    boolean has(String key);

    /**
     * 检查对象是否包含指定键且对应值为字符串
     *
     * @param key 要检查的键名
     * @return 如果包含且值为字符串则返回true
     */
    boolean hasString(String key);

    /**
     * 检查对象是否包含指定键且对应值为布尔值
     *
     * @param key 要检查的键名
     * @return 如果包含且值为布尔值则返回true
     */
    boolean hasBoolean(String key);

    /**
     * 检查对象是否包含指定键且对应值为数值
     *
     * @param key 要检查的键名
     * @return 如果包含且值为数值则返回true
     */
    boolean hasNumber(String key);

    /**
     * 检查对象是否包含指定键且对应值为数组
     *
     * @param key 要检查的键名
     * @return 如果包含且值为数组则返回true
     */
    boolean hasArray(String key);

    /**
     * 检查对象是否包含指定键且对应值为对象
     *
     * @param key 要检查的键名
     * @return 如果包含且值为对象则返回true
     */
    boolean hasObject(String key);

    /**
     * 创建新的空JSON对象节点实例
     *
     * @return 新的JSON对象节点实例
     */
    static JsonObjectNode jsonObjectNode()
    {
        return FACTORY.jsonObjectNode();
    }

    /**
     * 从指定的键值对Map创建新的JSON对象节点实例
     *
     * @param value 包含键值对的Map
     * @return 新的JSON节点实例
     */
    static JsonObjectNode jsonObjectNode(Map<String, ? extends JsonNode> value)
    {
        return FACTORY.jsonObjectNode(value);
    }
}
```
#### JsonParseException
```java
/*
 * 版权所有 (c) 2022-2024。PortSwigger Ltd. 保留所有权利。
 *
 * 此代码可用于扩展 Burp Suite Community Edition 和 Burp Suite Professional 的功能，
 * 前提是该使用不违反这些产品的许可条款。
 */

package burp.api.montoya.utilities.json;

/**
 * 表示在尝试解析无效JSON结构时抛出的异常
 */
public class JsonParseException extends JsonException
{
    /**
     * 使用指定的错误消息构造JSON解析异常
     * 
     * @param message 错误详情消息
     */
    public JsonParseException(String message)
    {
        super(message);
    }
}
```
#### JsonStringNode
```java
/*
 * 版权所有 (c) 2022-2024。PortSwigger Ltd. 保留所有权利。
 *
 * 此代码可用于扩展 Burp Suite Community Edition 和 Burp Suite Professional 的功能，
 * 前提是该使用不违反这些产品的许可条款。
 */

package burp.api.montoya.utilities.json;

import static burp.api.montoya.internal.ObjectFactoryLocator.FACTORY;

/**
 * 该接口用于定义JSON字符串节点
 */
public interface JsonStringNode extends JsonNode
{
    @Override
    String getValue();

    /**
     * 从指定字符串创建新的 {@link JsonStringNode} 实例
     *
     * @param value 字符串值
     * @return 新的 {@link JsonStringNode} 实例
     */
    static JsonStringNode jsonStringNode(String value)
    {
        return FACTORY.jsonStringNode(value);
    }
}
```
#### JsonUtils
```java
/*
 * 版权所有 (c) 2022-2024。PortSwigger Ltd. 保留所有权利。
 *
 * 此代码可用于扩展 Burp Suite Community Edition 和 Burp Suite Professional 的功能，
 * 前提是该使用不违反这些产品的许可条款。
 */

package burp.api.montoya.utilities.json;

/**
 * <p>
 * 该接口提供便捷方法来读取和操作JSON数据。
 * 所有方法都接受一个JSON字符串和位置参数，部分方法还接受额外的JSON参数用于修改原始JSON字符串。
 * </p>
 * 
 * <h2>位置语法说明：</h2>
 * <ul style="list-style-type: none">
 *     <li style="display: table-row"><div style="display: table-cell; padding-right: 10px"><b>.</b></div><div>使用点号分隔位置元素</div></li>
 *     <li style="display: table-row"><div style="display: table-cell; padding-right: 10px"><b>[n]</b></div><div>指定JSON数组中的第n个元素</div></li>
 *     <li style="display: table-row"><div style="display: table-cell; padding-right: 10px"><b>[]</b></div><div>指定JSON数组中的最后一个元素</div></li>
 *     <li style="display: table-row"><div style="display: table-cell; padding-right: 10px"><b>key</b></div><div>标识JSON对象中的键值对</div></li>
 * </ul>
 * <p><em>注意：索引从0开始计数。</em></p>
 * 
 * <p>示例（应用于下方JSON）：</p>
 * <p><em>account.[0].user.name</em> 将选中 "Peter Wiener"</p>
 * <p><em>account.[].user.name</em> 将选中 "Carlos Montoya"</p>
 * <p><em>account.[1].user.addresses.[]</em> 将选中 "Address 6"</p>
 * <pre>
 *     {
 *         "account": [
 *              {
 *                  "user": {
 *                      "name": "Peter Wiener",
 *                      "addresses": [
 *                          "Address 1",
 *                          "Address 2",
 *                          "Address 3"
 *                      ]
 *                  }
 *              },
 *              {
 *                  "user": {
 *                      "name": "Carlos Montoya",
 *                      "addresses": [
 *                          "Address 4",
 *                          "Address 5",
 *                          "Address 6"
 *                      ]
 *                  }
 *              }
 *         ]
 *     }
 * </pre>
 * 
 * <h2>注意事项</h2>
 * <p>
 * 接受JSON输入的方法允许使用单引号替代双引号。
 * </p>
 * <p>
 * 如需处理更复杂的场景，请参考 {@link JsonNode} 及其子类。
 * </p>
 */
public interface JsonUtils
{
    /**
     * 在源JSON的指定位置添加新JSON数据，返回修改后的新JSON字符串。
     *
     * @param sourceJson 待修改的源JSON字符串
     * @param location 标识新JSON数据的插入位置
     * @param newJson 要添加的新JSON数据
     * @return 修改后的新JSON字符串
     * @throws JsonException 当位置参数无效时抛出
     * @throws JsonParseException 当源JSON或新JSON格式无效时抛出
     */
    String add(String sourceJson, String location, String newJson);

    /**
     * 更新源JSON中指定位置的数据，返回修改后的新JSON字符串。
     *
     * @param sourceJson 待更新的源JSON字符串
     * @param location 标识需要更新的位置
     * @param newJson 用于替换的新JSON数据
     * @return 修改后的新JSON字符串
     * @throws JsonException 当位置参数无效时抛出
     * @throws JsonParseException 当源JSON或新JSON格式无效时抛出
     */
    String update(String sourceJson, String location, String newJson);

    /**
     * 移除源JSON中指定位置的数据，返回修改后的新JSON字符串。
     *
     * @param sourceJson 待修改的源JSON字符串
     * @param location 标识需要移除的数据位置
     * @return 修改后的新JSON字符串
     * @throws JsonException 当位置参数无效时抛出
     * @throws JsonParseException 当源JSON格式无效时抛出
     */
    String remove(String sourceJson, String location);

    /**
     * 从源JSON的指定位置读取数据，返回该位置的JSON字符串。
     *
     * @param sourceJson 待读取的源JSON字符串
     * @param location 标识需要读取的位置
     * @return 指定位置的JSON字符串
     * @throws JsonException 当位置参数无效时抛出
     * @throws JsonParseException 当源JSON格式无效时抛出
     */
    String read(String sourceJson, String location);

    /**
     * 从源JSON的指定位置读取布尔值。
     *
     * @param sourceJson 待读取的源JSON字符串
     * @param location 标识需要读取的位置
     * @return 指定位置的布尔值，若非布尔类型或不存在则返回null
     * @throws JsonException 当位置参数无效时抛出
     * @throws JsonParseException 当源JSON格式无效时抛出
     */
    Boolean readBoolean(String sourceJson, String location);

    /**
     * <p>从源JSON的指定位置读取数值并转为双精度浮点数。</p>
     *
     * <p><em>注意：Java中的double类型表示浮点数。</em></p>
     *
     * @param sourceJson 待读取的源JSON字符串
     * @param location 标识需要读取的位置
     * @return 指定位置的双精度数值，若非数值类型或不存在则返回null
     * @throws JsonException 当位置参数无效时抛出
     * @throws JsonParseException 当源JSON格式无效时抛出
     */
    Double readDouble(String sourceJson, String location);

    /**
     * <p>从源JSON的指定位置读取数值并转为长整型。</p>
     *
     * <p><em>注意：Java中的long类型表示整数 - 读取浮点数时会向下取整。</em></p>
     *
     * @param sourceJson 待读取的源JSON字符串
     * @param location 标识需要读取的位置
     * @return 指定位置的长整型数值，若非数值类型或不存在则返回null
     * @throws JsonException 当位置参数无效时抛出
     * @throws JsonParseException 当源JSON格式无效时抛出
     */
    Long readLong(String sourceJson, String location);

    /**
     * 从源JSON的指定位置读取字符串值。
     *
     * @param sourceJson 待读取的源JSON字符串
     * @param location 标识需要读取的位置
     * @return 指定位置的字符串值，若非字符串类型或不存在则返回null
     * @throws JsonException 当位置参数无效时抛出
     * @throws JsonParseException 当源JSON格式无效时抛出
     */
    String readString(String sourceJson, String location);

    /**
     * 检查输入的字符串是否能被解析为基本JSON类型（字符串、数字、布尔值、数组、对象或null）。
     *
     * <p><em>注意：传入null返回false，而"null"返回true。</em></p>
     * <p><em>注意：要传入JSON字符串，需使用单引号或双引号包裹（如"'foo'"或"\"foo\""）。</em></p>
     *
     * @param sourceJson 待检查的JSON字符串
     * @return 如果可解析为基本JSON类型则返回true，否则返回false（sourceJson为null时也返回false）
     */
    boolean isValidJson(String sourceJson);
}
```
### shell
#### ExecuteOptions
```java
package burp.api.montoya.utilities.shell;

import java.time.Duration;

import static burp.api.montoya.internal.ObjectFactoryLocator.FACTORY;

/**
 * 定义进程执行的配置选项
 */
public interface ExecuteOptions
{
    /**
     * 设置进程允许运行的最大时长（秒）。默认为10秒。设为0表示无超时限制。
     * <p>
     * 使用 {@link ExecuteOptions#withTimeoutBehavior} 定义超时行为（抛出异常或静默忽略）。
     *
     * @param seconds 超时时长（秒），0表示无限制
     * @return 更新了超时设置的 {@link ExecuteOptions} 实例
     */
    ExecuteOptions withTimeout(int seconds);

    /**
     * 设置进程允许运行的最大时长。默认为10秒。使用 {@link Duration#ZERO} 表示无超时限制。
     * <p>
     * 使用 {@link ExecuteOptions#withTimeoutBehavior} 定义超时行为（抛出异常或静默忽略）。
     *
     * @param duration 超时时长，{@code Duration.ZERO} 表示无限制
     * @return 更新了超时设置的 {@link ExecuteOptions} 实例
     */
    ExecuteOptions withTimeout(Duration duration);

    /**
     * 设置超时处理行为。默认为 {@link TimeoutBehavior#FAIL_ON_TIMEOUT}（超时抛出异常）。
     *
     * @param behavior 超时处理行为
     * @return 更新了超时行为的 {@link ExecuteOptions} 实例
     */
    ExecuteOptions withTimeoutBehavior(TimeoutBehavior behavior);

    /**
     * 设置标准错误(stderr)输出处理方式。默认为 {@link StderrBehavior#DISCARD}（丢弃错误输出）。
     *
     * @param behavior 标准错误处理行为
     * @return 更新了错误输出行为的 {@link ExecuteOptions} 实例
     */
    ExecuteOptions withStderrBehavior(StderrBehavior behavior);

    /**
     * 设置非零退出码处理行为。默认为 {@link ExitCodeBehavior#FAIL_ON_NON_ZERO}（非零退出码抛出异常）。
     *
     * @param behavior 非零退出码处理行为
     * @return 更新了退出码行为的 {@link ExecuteOptions} 实例
     */
    ExecuteOptions withExitCodeBehavior(ExitCodeBehavior behavior);

    /**
     * 定义进程使用的环境变量。
     *
     * @param name  变量名
     * @param value 变量值
     * @return 添加了环境变量的 {@link ExecuteOptions} 实例
     */
    ExecuteOptions withEnvironmentVariable(String name, String value);

    /**
     * 创建新的 {@link ExecuteOptions} 实例。
     *
     * @return 使用默认配置的 {@link ExecuteOptions} 实例
     */
    static ExecuteOptions executeOptions()
    {
        return FACTORY.executeOptions();
    }
}
```
#### ExitCodeBehavior
```java
package burp.api.montoya.utilities.shell;

/**
 * 定义如何处理非零退出码
 */
public enum ExitCodeBehavior
{
    /**
     * 如果进程返回非零退出码则抛出异常
     */
    FAIL_ON_NON_ZERO,

    /**
     * 静默忽略非零退出码
     */
    ALLOW_NON_ZERO
}
```
#### ProcessExecutionException
```java
package burp.api.montoya.utilities.shell;

/**
 * 表示在使用 {@link ShellUtils} 执行进程时发生错误的异常
 */
public class ProcessExecutionException extends RuntimeException
{
    /**
     * 使用指定的错误消息构造异常
     * @param message 错误详情消息
     */
    public ProcessExecutionException(String message)
    {
        super(message);
    }

    /**
     * 使用指定的错误消息和原因构造异常
     * @param message 错误详情消息
     * @param cause 导致此异常的根本原因
     */
    public ProcessExecutionException(String message, Throwable cause)
    {
        super(message, cause);
    }
}
```
#### ShellUtils
```java
package burp.api.montoya.utilities.shell;

/**
 * 提供从操作系统shell启动进程的实用工具
 */
public interface ShellUtils
{
    /**
     * 使用默认执行选项执行指定命令。按空白字符分割每个参数。如果需要保留空白字符，请考虑使用 {@link ShellUtils#execute(String...)}。
     * <p>
     * <b>警告：</b>避免将此方法用于任意输入。如果接受任意输入，请考虑使用 {@link ShellUtils#execute(String...)} 以最小化OS命令注入风险。
     * </p>
     *
     * @param command 命令及其参数，以空白字符分隔
     * @return 命令产生的输出
     */
    String dangerouslyExecute(String command);

    /**
     * 使用指定执行选项执行指定命令。按空白字符分割每个参数。如果需要保留空白字符，请考虑使用 {@link ShellUtils#execute(ExecuteOptions, String...)}。
     * <p>
     * <b>警告：</b>避免将此方法用于任意输入。如果接受任意输入，请考虑使用 {@link ShellUtils#execute(String...)} 以最小化OS命令注入风险。
     * </p>
     *
     * @param options 控制命令执行方式的选项
     * @param command 命令及其参数，以空白字符分隔
     * @return 命令产生的输出
     */
    String dangerouslyExecute(ExecuteOptions options, String command);

    /**
     * 使用默认执行选项执行指定命令
     *
     * @param command 命令及其参数，以单独字符串形式指定
     * @return 命令产生的输出
     */
    String execute(String... command);

    /**
     * 使用指定执行选项执行指定命令
     *
     * @param options 控制命令执行方式的选项
     * @param command 命令及其参数，以单独字符串形式指定
     * @return 命令产生的输出
     */
    String execute(ExecuteOptions options, String... command);
}
```
#### StderrBehavior
```java
package burp.api.montoya.utilities.shell;

/**
 * 定义标准错误输出(stderr)的处理方式
 */
public enum StderrBehavior
{
    /**
     * 将stderr输出合并到stdout流中
     */
    MERGE,

    /**
     * 丢弃所有stderr输出
     */
    DISCARD
}
```
#### TimeoutBehavior
```java
package burp.api.montoya.utilities.shell;

/**
 * 定义进程超时时的处理方式
 */
public enum TimeoutBehavior
{
    /**
     * 如果进程超时则抛出异常
     */
    FAIL_ON_TIMEOUT,

    /**
     * 静默忽略进程执行超时
     */
    ALLOW_TIMEOUT
}
```
## websocket
### BinaryMessage
```java
/*
 * 版权所有 (c) 2022-2023。PortSwigger Ltd. 保留所有权利。
 *
 * 此代码可用于扩展Burp Suite社区版和Burp Suite专业版的功能，
 * 前提是该使用不违反这些产品的许可条款。
 */

package burp.api.montoya.websocket;

import burp.api.montoya.core.ByteArray;

/**
 * WebSocket二进制消息接口
 */
public interface BinaryMessage
{
    /**
     * @return 二进制格式的WebSocket消息内容
     */
    ByteArray payload();

    /**
     * @return 消息的传输方向
     */
    Direction direction();
}
```
### BinaryMessageAction
```java
/*
 * 版权所有 (c) 2023。PortSwigger Ltd. 保留所有权利。
 *
 * 此代码可用于扩展Burp Suite社区版和Burp Suite专业版的功能，
 * 前提是该使用不违反这些产品的许可条款。
 */

package burp.api.montoya.websocket;

import burp.api.montoya.core.ByteArray;

import static burp.api.montoya.internal.ObjectFactoryLocator.FACTORY;

/**
 * WebSocket二进制消息操作接口
 */
public interface BinaryMessageAction
{
    /**
     * @return 当前消息关联的操作类型
     */
    MessageAction action();

    /**
     * @return 消息的二进制有效载荷
     */
    ByteArray payload();

    /**
     * 构建待处理的WebSocket二进制消息
     *
     * @param payload 二进制消息内容
     * @return 包含待处理消息的 {@link BinaryMessageAction}
     */
    static BinaryMessageAction continueWith(ByteArray payload)
    {
        return FACTORY.continueWithBinaryMessage(payload);
    }

    /**
     * 构建待处理的WebSocket二进制消息
     *
     * @param binaryMessage 二进制消息对象
     * @return 包含待处理消息的 {@link BinaryMessageAction}
     */
    static BinaryMessageAction continueWith(BinaryMessage binaryMessage)
    {
        return FACTORY.continueWithBinaryMessage(binaryMessage.payload());
    }

    /**
     * 构建将被丢弃的WebSocket二进制消息
     *
     * @return 丢弃消息的 {@link BinaryMessageAction}
     */
    static BinaryMessageAction drop()
    {
        return FACTORY.dropBinaryMessage();
    }

    /**
     * 构建WebSocket二进制消息操作
     *
     * @param payload 二进制消息内容
     * @param action 要对消息执行的操作
     * @return 包含消息和操作的 {@link BinaryMessageAction}
     */
    static BinaryMessageAction binaryMessageAction(ByteArray payload, MessageAction action)
    {
        return FACTORY.binaryMessageAction(payload, action);
    }
}
```
### Direction
```java
/*
 * 版权所有 (c) 2022-2023。PortSwigger Ltd. 保留所有权利。
 *
 * 此代码可用于扩展Burp Suite社区版和Burp Suite专业版的功能，
 * 前提是该使用不违反这些产品的许可条款。
 */

package burp.api.montoya.websocket;

/**
 * 该枚举用于表示WebSocket消息的传输方向
 */
public enum Direction
{
    /**
     * 从客户端发往服务端的消息
     */
    CLIENT_TO_SERVER,
    
    /**
     * 从服务端发往客户端的消息
     */
    SERVER_TO_CLIENT
}
```
### MessageAction
```java
/*
 * 版权所有 (c) 2023。PortSwigger Ltd. 保留所有权利。
 *
 * 此代码可用于扩展Burp Suite社区版和Burp Suite专业版的功能，
 * 前提是该使用不违反这些产品的许可条款。
 */

package burp.api.montoya.websocket;

/**
 * 表示应用于 {@link TextMessageAction} 或 {@link BinaryMessageAction} 的操作枚举
 */
public enum MessageAction
{
    /**
     * 指示Burp继续转发该消息
     */
    CONTINUE,

    /**
     * 指示Burp丢弃该消息
     */
    DROP
}
```
### MessageHandler
```java
/*
 * 版权所有 (c) 2022-2023。PortSwigger Ltd. 保留所有权利。
 *
 * 此代码可用于扩展Burp Suite社区版和Burp Suite专业版的功能，
 * 前提是该使用不违反这些产品的许可条款。
 */

package burp.api.montoya.websocket;

/**
 * 此接口允许扩展在接收到WebSocket消息或连接关闭时收到通知。
 */
public interface MessageHandler
{
    /**
     * 当从应用程序收发文本消息时调用。
     * 扩展可在此修改消息内容，然后才发送给应用程序或由Burp处理。
     *
     * @param textMessage 被拦截的文本格式WebSocket消息
     * @return 处理后的消息
     */
    TextMessageAction handleTextMessage(TextMessage textMessage);

    /**
     * 当从应用程序收发二进制消息时调用。
     * 扩展可在此修改消息内容，然后才发送给应用程序或由Burp处理。
     *
     * @param binaryMessage 被拦截的二进制格式WebSocket消息
     * @return 处理后的消息
     */
    BinaryMessageAction handleBinaryMessage(BinaryMessage binaryMessage);

    /**
     * 当WebSocket连接关闭时调用。
     */
    default void onClose()
    {
    }
}
```
### TextMessage
```java
/*
 * 版权所有 (c) 2022-2023。PortSwigger Ltd. 保留所有权利。
 *
 * 此代码可用于扩展Burp Suite社区版和Burp Suite专业版的功能，
 * 前提是该使用不违反这些产品的许可条款。
 */

package burp.api.montoya.websocket;

/**
 * WebSocket文本消息接口
 */
public interface TextMessage
{
    /**
     * @return 文本格式的WebSocket消息内容
     */
    String payload();

    /**
     * @return 消息的传输方向
     */
    Direction direction();
}
```
### TextMessageAction
```java
/*
 * 版权所有 (c) 2023。PortSwigger Ltd. 保留所有权利。
 *
 * 此代码可用于扩展Burp Suite社区版和Burp Suite专业版的功能，
 * 前提是该使用不违反这些产品的许可条款。
 */

package burp.api.montoya.websocket;

import static burp.api.montoya.internal.ObjectFactoryLocator.FACTORY;

/**
 * WebSocket文本消息操作接口
 */
public interface TextMessageAction
{
    /**
     * @return 当前消息关联的操作类型
     */
    MessageAction action();

    /**
     * @return 消息的有效载荷内容
     */
    String payload();

    /**
     * 构建待处理的WebSocket文本消息
     *
     * @param payload 文本消息内容
     * @return 包含待处理消息的 {@link TextMessageAction}
     */
    static TextMessageAction continueWith(String payload)
    {
        return FACTORY.continueWithTextMessage(payload);
    }

    /**
     * 构建待处理的WebSocket文本消息
     *
     * @param textMessage 文本消息对象
     * @return 包含待处理消息的 {@link TextMessageAction}
     */
    static TextMessageAction continueWith(TextMessage textMessage)
    {
        return FACTORY.continueWithTextMessage(textMessage.payload());
    }

    /**
     * 构建将被丢弃的WebSocket文本消息
     *
     * @return 丢弃消息的 {@link TextMessageAction}
     */
    static TextMessageAction drop()
    {
        return FACTORY.dropTextMessage();
    }

    /**
     * 构建WebSocket文本消息操作
     *
     * @param payload 消息内容
     * @param action 要对消息执行的操作
     * @return 包含消息和操作的 {@link TextMessageAction}
     */
    static TextMessageAction textMessageAction(String payload, MessageAction action)
    {
        return FACTORY.textMessageAction(payload, action);
    }
}
```
### WebSocket
```java
/*
 * 版权所有 (c) 2022-2023。PortSwigger Ltd. 保留所有权利。
 *
 * 此代码可用于扩展Burp Suite社区版和Burp Suite专业版的功能，
 * 前提是该使用不违反这些产品的许可条款。
 */

package burp.api.montoya.websocket;

import burp.api.montoya.core.ByteArray;
import burp.api.montoya.core.Registration;

/**
 * Burp中的WebSocket连接接口
 */
public interface WebSocket
{
    /**
     * 发送文本消息到WebSocket连接
     *
     * @param message 要发送的文本消息
     */
    void sendTextMessage(String message);

    /**
     * 发送二进制消息到WebSocket连接
     *
     * @param message 要发送的二进制消息
     */
    void sendBinaryMessage(ByteArray message);

    /**
     * 关闭WebSocket连接
     */
    void close();

    /**
     * 注册消息处理器，用于处理WebSocket消息收发
     *
     * @param handler 扩展实现的 {@link MessageHandler} 接口实例
     * @return 处理器的 {@link Registration} 注册对象
     */
    Registration registerMessageHandler(MessageHandler handler);
}
```
### WebSocketCreated
```java
/*
 * 版权所有 (c) 2022-2023。PortSwigger Ltd. 保留所有权利。
 *
 * 此代码可用于扩展Burp Suite社区版和Burp Suite专业版的功能，
 * 前提是该使用不违反这些产品的许可条款。
 */

package burp.api.montoya.websocket;

import burp.api.montoya.core.ToolSource;
import burp.api.montoya.http.message.requests.HttpRequest;

/**
 * WebSocket连接创建信息接口
 */
public interface WebSocketCreated
{
    /**
     * @return 已创建的WebSocket连接实例
     */
    WebSocket webSocket();

    /**
     * @return 触发WebSocket创建的HTTP升级请求
     */
    HttpRequest upgradeRequest();

    /**
     * @return 创建该WebSocket连接的Burp工具来源
     */
    ToolSource toolSource();
}
```
### WebSocketCreatedHandler
```java
/*
 * 版权所有 (c) 2022-2023。PortSwigger Ltd. 保留所有权利。
 *
 * 此代码可用于扩展Burp Suite社区版和Burp Suite专业版的功能，
 * 前提是该使用不违反这些产品的许可条款。
 */

package burp.api.montoya.websocket;

/**
 * 扩展可实现此接口并通过调用 {@link WebSockets#registerWebSocketCreatedHandler} 注册WebSocket处理器。<br>
 * 当任何Burp工具创建新WebSocket连接时，该处理器将收到通知。
 */
public interface WebSocketCreatedHandler
{
    /**
     * 当应用程序WebSocket连接创建完成时由Burp调用。
     *
     * @param webSocketCreated 包含正在创建的应用程序WebSocket相关信息的 {@link WebSocketCreated} 对象
     */
    void handleWebSocketCreated(WebSocketCreated webSocketCreated);
}
```
### WebSockets
```java
/*
 * 版权所有 (c) 2022-2023。PortSwigger Ltd. 保留所有权利。
 *
 * 此代码可用于扩展Burp Suite社区版和Burp Suite专业版的功能，
 * 前提是该使用不违反这些产品的许可条款。
 */

package burp.api.montoya.websocket;

import burp.api.montoya.core.Registration;
import burp.api.montoya.http.HttpService;
import burp.api.montoya.http.message.requests.HttpRequest;
import burp.api.montoya.websocket.extension.ExtensionWebSocketCreation;

/**
 * 提供对Burp中WebSocket相关功能的访问
 */
public interface WebSockets
{
    /**
     * 注册处理器，当任何Burp工具创建WebSocket时将被调用
     *
     * @param handler 扩展实现的 {@link WebSocketCreatedHandler} 接口实例
     * @return 处理器的 {@link Registration} 注册对象
     */
    Registration registerWebSocketCreatedHandler(WebSocketCreatedHandler handler);

    /**
     * 使用指定的服务和路径创建新的WebSocket连接
     *
     * @param service 指定目标主机的 {@link HttpService}
     * @param path 升级HTTP请求的路径
     * @return {@link ExtensionWebSocketCreation} 创建结果
     */
    ExtensionWebSocketCreation createWebSocket(HttpService service, String path);

    /**
     * 使用指定的升级请求创建新的WebSocket连接
     *
     * @param upgradeRequest 升级用的 {@link HttpRequest} 请求
     * @return {@link ExtensionWebSocketCreation} 创建结果
     */
    ExtensionWebSocketCreation createWebSocket(HttpRequest upgradeRequest);
}
```
### extension
#### ExtensionWebSocket
```java
/*
 * 版权所有 (c) 2023。PortSwigger Ltd. 保留所有权利。
 *
 * 此代码可用于扩展Burp Suite社区版和Burp Suite专业版的功能，
 * 前提是该使用不违反这些产品的许可条款。
 */

package burp.api.montoya.websocket.extension;

import burp.api.montoya.core.ByteArray;
import burp.api.montoya.core.Registration;

/**
 * 通过扩展API创建的WebSocket连接
 */
public interface ExtensionWebSocket
{
    /**
     * 允许扩展通过WebSocket发送文本消息
     *
     * @param message 要发送的文本消息
     */
    void sendTextMessage(String message);

    /**
     * 允许扩展通过WebSocket发送二进制消息
     *
     * @param message 要发送的二进制消息
     */
    void sendBinaryMessage(ByteArray message);

    /**
     * 关闭WebSocket连接
     */
    void close();

    /**
     * 注册消息处理器，用于接收来自服务端的消息通知
     *
     * @param handler 扩展实现的 {@link ExtensionWebSocketMessageHandler} 接口实例
     * @return 处理器的 {@link Registration} 注册对象
     */
    Registration registerMessageHandler(ExtensionWebSocketMessageHandler handler);
}
```
#### ExtensionWebSocketCreation
```java
/*
 * 版权所有 (c) 2023。PortSwigger Ltd. 保留所有权利。
 *
 * 此代码可用于扩展Burp Suite社区版和Burp Suite专业版的功能，
 * 前提是该使用不违反这些产品的许可条款。
 */

package burp.api.montoya.websocket.extension;

import burp.api.montoya.http.message.responses.HttpResponse;

import java.util.Optional;

/**
 * WebSocket连接创建尝试的结果
 */
public interface ExtensionWebSocketCreation
{
    /**
     * 获取WebSocket连接创建尝试的状态。
     *
     * @return {@link ExtensionWebSocketCreationStatus} 创建状态
     */
    ExtensionWebSocketCreationStatus status();

    /**
     * 获取已创建的WebSocket连接。
     *
     * @return 已创建的 {@link ExtensionWebSocket}，可能为空
     */
    Optional<ExtensionWebSocket> webSocket();

    /**
     * 获取WebSocket创建尝试的HTTP响应。
     *
     * @return {@link HttpResponse} 升级响应，可能为空
     */
    Optional<HttpResponse> upgradeResponse();
}
```
#### ExtensionWebSocketCreationStatus
```java
/*
 * 版权所有 (c) 2023。PortSwigger Ltd. 保留所有权利。
 *
 * 此代码可用于扩展Burp Suite社区版和Burp Suite专业版的功能，
 * 前提是该使用不违反这些产品的许可条款。
 */

package burp.api.montoya.websocket.extension;

/**
 * WebSocket连接创建尝试的状态枚举
 */
public enum ExtensionWebSocketCreationStatus
{
    /**
     * WebSocket连接创建成功
     */
    SUCCESS,

    /**
     * 指定的主机无效
     */
    INVALID_HOST,

    /**
     * 无法解析指定主机的地址
     */
    UNKNOWN_HOST,

    /**
     * 指定的端口无效
     */
    INVALID_PORT,

    /**
     * 无法连接到指定主机
     */
    CONNECTION_FAILED,

    /**
     * 指定的升级请求无效
     */
    INVALID_REQUEST,

    /**
     * 服务器返回了非升级响应
     */
    NON_UPGRADE_RESPONSE,

    /**
     * 指定的端点配置为流式响应
     */
    STREAMING_RESPONSE
}
```
#### ExtensionWebSocketMessageHandler
```java
/*
 * 版权所有 (c) 2023。PortSwigger Ltd. 保留所有权利。
 *
 * 此代码可用于扩展Burp Suite社区版和Burp Suite专业版的功能，
 * 前提是该使用不违反这些产品的许可条款。
 */

package burp.api.montoya.websocket.extension;

import burp.api.montoya.websocket.BinaryMessage;
import burp.api.montoya.websocket.TextMessage;

/**
 * 此接口允许扩展在接收到WebSocket消息或连接关闭时收到通知。
 */
public interface ExtensionWebSocketMessageHandler
{
    /**
     * 当从应用程序接收到文本消息时调用。
     *
     * @param textMessage 文本格式的WebSocket消息
     */
    void textMessageReceived(TextMessage textMessage);

    /**
     * 当从应用程序接收到二进制消息时调用。
     *
     * @param binaryMessage 二进制格式的WebSocket消息
     */
    void binaryMessageReceived(BinaryMessage binaryMessage);

    /**
     * 当WebSocket连接关闭时调用。
     */
    default void onClose()
    {
    }
}
```
