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
###
```java

```
###
```java

```
###
```java

```
###
```java

```
