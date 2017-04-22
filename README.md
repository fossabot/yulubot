# quote-bot

A Telegram Bot for Managing Quotes

思路：

Public Channel的对话是有对应的URL的。即ChannelURL/<id>的形式，如https://t.me/Y2xhcmt6ancncyBwZXJzb25hbCBjaGFu/7

所以首先要把语录Channel中的每条消息以及对应的URL先存起来，然后用户查询的时候对消息进行全文检索。bot返回消息的URL。

如果直接在与其他用户的对话中使用bot进行inline查询，那么点击后直接发出了语录的URL。这样是不合适的。

所以可以引导用户与bot私聊进行查询。这样就不需要inline了。在与bot私聊界面返回对应的URL，然后用户点击跳转到对应的语录消息，然后转发出来即可。

如果inline查询的时候点击inline的result能够直接把消息转发出来就好了，但是似乎不可行。**待验证**

也可以支持搜索某个用户的所有语录消息。反正Telegram有链接的预览。但是如果某个用户的所有语录消息的数量太多，全部返回给用户显然是不合适的，因此可以分页。

改进：

以后，用户依然转发消息到原来的语录中。然后bot监听语录中的消息（因此bot需要被加入到Channel中），遇到新的消息加入到数据库中。

当然，也可以做一些统计。如语录中谁的消息最多，谁转发的最多，哪条消息被查询的次数最多，哪个用户查询的次数最多，等等。

由于似乎Private Channel无法以ChannelURL+ID的形式访问消息，所以语录Channel必须是Public的。那么对于bot的使用是否有必要进行认证？如何认证？

特色：

由于图像等也可以通过Channel的URL访问到，所以在图像上也可以做文章。比如提取一些图像的特征作为图像消息的查询参数关键字等。有比如假如图像中有文字，可以进行OCR。这种场景主要是针对之前见过这幅图的人，或者是这条语录消息的转发者。当然如果有这些额外的消息，应该在转发者转发完之后就回复告诉他。

至于是否要做一个Web页面来管理和查看，待定。

获取Channel中的历史消息并同步到bot的数据库

使用telegram history dump https://github.com/tvdstaaij/telegram-history-dump

使用mongo-connecter同步mongodb和elasticsearch之间的数据

使用elasticsearch来响应客户端的查询请求，因为mongodb对中文分词的支持比较差，且是商业版才支持

是否有必要采用redis做cache？

直接转发API：

POST https://api.telegram.org/bot<token>/forwardMessage chat_id=<target chat id> from_chat_id=<from chat id> message_id=<id>
