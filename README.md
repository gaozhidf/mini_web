miniweb
Sketch:

	基于C gnu99和Linux socket API实现的http服务器
    	1.支持多个客户端连接
    	2.支持HTTP协议中的GET方法
    	3.支持通过浏览器访问多种类型的文件资源，如.jpg和.html
    	4.支持配置文件
    	5.支持日志记录
Struct:
  
  	1.服务端结构体
	typedef struct {
    	// 包含日志文件
    	FILE *logfp;
    	// 监听的socket
    	int sockfd;
    	// 监听的端口
    	short port;
    	// 是否使用日志文件
    	int use_logfile;
    	// 是否以守护进程的方式启动
    	int is_daemon;
    	// 是否chroot
    	int do_chroot;
    	// 配置信息
    	config *conf;
} server;
   	2.连接后的服务端结构体
   	typedef struct {
    	// 客户端连接的socket
    	int sockfd;
    	// 状态码
    	int status_code;
    	// 接收队列
    	string *recv_buf;
    	// HTTP请求
    	http_request *request;
    	// HTTP响应
    	http_response *response;
    	// 接收状态
    	http_recv_state recv_state;
    	// 客户端地址信息
    	struct sockaddr_in addr;
    	// 请求长度
    	size_t request_len;
    	// 请求文件的真实路径
    	char real_path[PATH_MAX];
} connection;
    	3.HTTP请求的结构体
	typedef struct {
    	http_method method;
    	http_version version;
    	char *method_raw;
    	char *version_raw;
    	char *uri;
    	http_headers *headers;
    	int content_length;
} http_request;
    	4.HTTP响应结构体
	typedef struct {
    	int content_length;
    	string *entity_body;
    	http_headers *headers;
} http_response;
