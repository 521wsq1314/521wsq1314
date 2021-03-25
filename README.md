<!DOCTYPE html>
<html lang="en">

<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
    <script src="http://www.jq22.com/jquery/1.11.1/jquery.min.js"></script>

    <script src="http://www.jq22.com/jquery/bootstrap-3.3.4.js"></script>
    <link href="http://www.jq22.com/jquery/bootstrap-3.3.4.css" rel="stylesheet">
    <script src="../bootstrap/01-jquery.slim.min.js"></script>
    <link rel="stylesheet" href="../bootstrap/02-bootstrap.min.css">
    <script src="../bootstrap/03-bootstrap.min.js"></script>
    <script src="../js/jqurey.min.js"></script>
    <link rel="stylesheet" href="./bianji.css">
    <link rel="stylesheet" href="../css/bianji.css">
    <!-- <script src="./jQueryDistpicker20160621/js/distpicker.data.js"></script>
    <script src="./jQueryDistpicker20160621/js/distpicker.js"></script>
    <script src="./jQueryDistpicker20160621/js/main.js"></script> -->
    <script src="../jQueryDistpicker20160621/js/distpicker.data.js"></script>
    <script src="../jQueryDistpicker20160621/js/distpicker.js"></script>
    <script src="../jQueryDistpicker20160621/js/main.js"></script>

</head>

<body>
    <div class=" wrapper ">
        <ul class="admin ">
            <a href=" " class="zuo "><img src="./微信图片_20210322105629.jpg" alt=" "></a>
            <a href=" " class="you "></a>
        </ul>
        <div class="student ">
            <ul class="xygl ">
                <li>学员管理</li>
                <li>讲师管理</li>
                <li>助教管理</li>
            </ul>
            <div class="zhuang ">
                <p class="xue "><span>学员管理</span>><span>新增/编辑学员</span></p>
                <div class="da ">
                    <p class="ji ">基本信息</p>
                    <ul class="xuesheng ">
                        <li>
                            <p> <span>*</span>学生名称：</p><input type="text " name=" " id="nickname" class="inp1 ">

                        </li>
                        <li>
                            <p> <span>*</span>学生手机号：</p><input type="text" maxlength="11" id="mobel" name="phone" onkeypress="intOnly()" style="ime-mode:Disabled" />

                        </li>
                        <li>
                            <p>学员头像：</p>
                            <div class="tou ">
                                <i class="dd"></i>

                                <button>+选择文件 <input type="file" id="xuan"> </button>

                                <p>300*300像素或1：1支持jpg、jpeg、png小于5M</p>
                            </div>
                        </li>
                        <li>
                            <p>性别：</p>
                            <div class="btn-group ">
                                <select name="" id="sex">
                                    <option value="0">男</option>
                                    <option value="1">女</option>
                                </select>
                            </div>
                        </li>
                        <li>
                            <p>出生日期：</p>
                            <input type="date" name=" " id="birthdy">

                        </li>
                        <li>
                            <p>所在城市：</p>
                            <form class="form-inline">
                                <div data-toggle="distpicker">
                                    <div class="form-group">
                                        <label class="sr-only" for="province1">Province</label>
                                        <select class="form-control" id="province1"></select>
                                    </div>
                                    <div class="form-group">
                                        <label class="sr-only" for="city1">City</label>
                                        <select class="form-control" id="city1"></select>
                                    </div>
                                    <div class="form-group">
                                        <label class="sr-only" for="district1">District</label>
                                        <select class="form-control" id="district1"></select>
                                    </div>
                                </div>
                            </form>
                        </li>
                    </ul>
                    <button id="bx">保存</button>
                </div>
            </div>
        </div>
    </div>
</body>
<script src="../bian.js"></script>

</html>
<script>
    if (localStorage.getItem("userInfo")) {
        var token = JSON.parse(localStorage.getItem("userInfo")).remember_token;
    } else {

        location.assign("../网校")
    }
    var path, shengId, shiId, xianId
    $("input[type='file']").change(function() {
        var file = this.files[0];
        if (window.FileReader) {
            var reader = new FileReader();
            reader.readAsDataURL(file);
            //监听文件读取结束后事件    
            reader.onloadend = function(e) {
                $(".dd").css("background-image", "url(" + e.target.result + ")");
                //e.target.result就是最后的路径地址
            };
        }




        var sendData = new FormData();
        console.log(sendData);
        sendData.append("file", this.files[0]);
        $.ajax({
            headers: {
                Authorization: "Bearer" + token
            },
            url: "http://120.53.31.103:84/api/public/img",
            data: sendData,
            type: "post",
            processData: false,
            contentType: false,
            success: function(res) {
                if (res.conde == 200) {
                    path = res.data.path
                }
            }
        })
    })

    $('#province1').on('change', function() {
        shengId = $('#province1 option:selected').attr('data-code')
    })
    $('#city1').on('change', function() {
        shiId = $('#city1 option:selected').attr('data-code')
    })
    $('#district1').on('change', function() {
        xianId = $('#district1 option:selected').attr('data-code')
    })
    $('#bx').click(function() {





        $.ajax({
            headers: {
                Authorization: "Bearer" + token
            },
            url: "http://120.53.31.103:84/api/user",
            data: {
                attr: [],
                avatar: path,
                birthday: $('#birthdy').val(),
                city_id: shiId,
                district_id: xianId,
                mobile: $('#mobel').val(),
                nickname: $('#nickname').val(),
                province_id: shengId,
                sex: $('#sex').val(),
                user_attr: "[]"
            },
            type: "POST",
            dataType: "json",
            success: function(res) {
                if (res.conde == 200) {
                    location.assign('./demo.html')
                }
            }
        })
    })
</script>
