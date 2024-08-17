productName" : " Trình cài đặt BetterDiscord " ,
  "description" : " Một chương trình độc lập đơn giản tự động cài đặt, gỡ bỏ và bảo trì BetterDiscord . "
  "tác giả" : " BetterDiscord " ,
  "phiên bản" : " 1. 2.1 " ,
  "phiên bản" : " 1. 3.0 " ,
  "giấy phép" : " MIT " ,
  "script" : {
    "dev" : " dev electron-webpack " ,
  76 thay đổi: 59 bổ sung & 17 xóa76 
src/renderer/actions/install.js
Số dòng tập tin gốc	Số dòng khác biệt	Thay đổi dòng khác biệt
@@ -49,35 +49,77 @@ hàm bất đồng bộ makeDirectories(...folders) {

const  getJSON  =  phin.defaults ( { phương thức : "GET" , phân tích cú pháp : " json " , followRedirects : đúng , tiêu đề : { "User-Agent" : "BetterDiscord/Installer" } } ) ;   
const  downloadFile  =  phin.defaults ( { phương thức : "GET" , followRedirects : đúng , tiêu đề : { "User-Agent" : "BetterDiscord/Installer" , "Chấp nhận" : " application/octet-stream " } } ) ;   
const  asarPath  =  đường dẫn . join ( bdDataFolder ,  "betterdiscord.asar" ) ;
 hàm  async tải xuốngAsar ( )  {
    thử  {
        const  response  =  await  downloadFile ( "https://betterdiscord.app/Download/betterdiscord.asar" )
        const  bdVersion  =  phản hồi . tiêu đề [ "x-bd-version" ] ;
        nếu  ( 200  < =  response.statusCode && response.statusCode < 300 ) {     
            log ( `✅ Đã tải xuống phiên bản BetterDiscord ${ bdVersion } từ trang web chính thức` ) ;
            trả về  phản hồi . body ;
        }
        throw  new  Error ( `Mã trạng thái không chỉ ra thành công: ${ response . statusCode } ` ) ;
    }
    bắt  ( lỗi )  {
        log ( `❌ Không tải được gói từ trang web chính thức` ) ;
        nhật ký ( `❌ ${ lỗi . thông báo } ` ) ;
        log ( `Quay lại GitHub...` ) ;
    }
    để  assetUrl ;
    hãy để  bdVersion ;
    thử  {
        const  response  =  await  getJSON ( RELEASE_API ) ;
        const  phát hành  =  phản hồi . body ;
        const  asset  =  phát hành  &&  phát hành . length  &&  phát hành [ 0 ] . asset  &&  phát hành [ 0 ] . asset . find ( a  =>  a . name . toLowerCase ( )  ===  "betterdiscord.asar" ) ;
        const  assetsUrl  =  nội dung  &&  nội dung . địa chỉ ;
        assetsUrl  =  nội dung  &&  nội dung . địa chỉ ;
        bdVersion  =  tài sản  &&  phát hành [ 0 ] .tag_name ;
        nếu  ( ! assetUrl )  {
