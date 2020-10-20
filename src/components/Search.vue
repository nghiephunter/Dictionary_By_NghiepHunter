<template>
  <div id="app">
    <div class="diction" v-if="main==false">
      <button @click="display=true;main=true">
      <span></span><span></span><span></span><span></span>
      Dictionary</button>
      <button @click="display=true;main=true">
      <span></span><span></span><span></span><span></span>
      By</button>
      <button @click="display=true;main=true">
      <span></span><span></span><span></span><span></span>
      Nghiệp Hunter</button>
    </div>

    <div class="uses" v-if="display==true">
      <button @click="process=1;display=false">
        <span></span><span></span><span></span><span></span>
        Tra từ
      </button>
      <button @click="process=2;display=false">
        <span></span><span></span><span></span><span></span>
        Xóa từ</button>
        <button @click="process=3;display=false">
        <span></span><span></span><span></span><span></span>
        History</button>
      <button @click="main=false;display=false">
        <span></span><span></span><span></span><span></span>
        Back</button>
    </div>

    <div class="search" v-if="process==1">
      <div id="load" v-if="handing == 0">
        <input type="text" name="" v-model="text" placeholder="Nhập từ cần tra...">
        <button @click="search(); handing=1; history()">
          <span></span><span></span><span></span><span></span>
        Search</button>
        <button @click="display=true; process=null">
          <span></span><span></span><span></span><span></span>
          Back</button>
      </div>

      <div id="processing" v-if="handing==1">
        <div v-if="search()!=null">
          <span>DỊCH NGHĨA TỪ</span>
          <br>
          <span>{{text}}: {{target}}</span>
        <button @click="handing=0; text=null">
          <span></span><span></span><span></span><span></span>
          Back</button>
        </div>
        <div v-if="search()==null">
          <span>Không có từ này trong data!</span>
          <br>
          <span>Bạn có muốn thêm từ vào data ?</span>
          <button @click="select=1;req=false;text=null">
            <span></span><span></span><span></span><span></span>
            Yes</button>
          <div v-if="select==1">
            <input type="text" name="" placeholder="Tiếng anh..." v-model="english">
            <input type="tex" name="" placeholder="Tiếng việt..." v-model="vietnamese">
            <button @click="select=0; add();english=null;vietnamese=null;req=true">
              <span></span><span></span><span></span><span></span>
              Enter</button>
          </div>
          <span v-show="req == true">Thêm từ thành công!</span>
          <button @click="handing=0; text=null;select=0;english=null;vietnamese=null;req=false">
            <span></span><span></span><span></span><span></span>
            Back</button>
        </div>
      </div>
    </div>


    <div class="deleteW" v-if="process==2">
      <div v-if="flag==0">
        <span>Nhập từ cần xóa</span>
        <input type="text" name="" placeholder="English..." v-model="english">
        <button @click="deleteWord(); flag=1; english=null">
          <span></span><span></span><span></span><span></span>
          Delete</button>
      </div>
      <div v-if="flag==1">
        <span>Xóa từ thành công!</span>
        <button @click="flag=0; english=null">
          <span></span><span></span><span></span><span></span>
          Tiếp tục xóa</button>
      </div>
      <button @click="display=true; process=null;flag=0;english=null">
        <span></span><span></span><span></span><span></span>
        Back</button>
    </div>

    <div class="wordhis" v-if="process==3">
      <button @click="display=true; process=null">
        <span></span><span></span><span></span><span></span>
        Back</button>
      <div id="hisSub">
        <span>Danh sách từ đã tra cứu</span>
        <br>
        <div v-for="(wordHis,index) in his" :key="index">
        {{index+1}}. {{wordHis.eng}}: {{wordHis.vie}}
        <br>
        </div>
      </div>
    </div>
  </div>
</template>
<script>
export default {
  data(){
    return{
      req: false,
      main: false,
      display: false,
      process: null,
      english: null,
      vietnamese: null,
      select: 0,
      flag: 0,
      handing: 0,
      target: null,
      text: null,
      his: [

      ],
      word: [
      { eng: 'able', vie: 'có năng lực, có tài' },
      { eng: 'abandon',vie: 'bỏ, từ bỏ'},
      { eng: 'about', vie: 'khoảng, về'},
      { eng: 'above', vie: 'ở trên, lên trên' },
      { eng: 'act', vie: 'hành động, hành vi, cử chỉ, đối xử' },
      { eng: 'add', vie: 'cộng, thêm vào' },
      { eng: 'afraid', vie: 'sợ, sợ hãi, hoảng sợ' },
      { eng: 'after', vie: 'sau, đằng sau, sau khi' },
      { eng: 'again', vie: 'lại, nữa, lần nữa' },
      { eng: 'against', vie: 'chống lại, phản đối' },
      { eng: 'age', vie: 'tuổi' },
      { eng: 'ago',vie: 'trước đây' },
      { eng: 'agree',vie: 'đồng ý, tán thành' },
      { eng: 'air',vie: 'không khí, bầu không khí, không gian' },
      { eng: 'all',vie: 'tất cả'},
      { eng: 'allow',vie: 'cho phép, để cho' },
      { eng: 'also',vie: 'cũng, cũng vậy, cũng thế' },
      { eng: 'always',vie: 'luôn luôn' },
      { eng: 'among',vie: 'giữa, ở giữa' },
      { eng: 'an',vie: 'nếu' },
      { eng: 'and',vie: 'và' },
      { eng: 'anger',vie: 'sự tức giận, sự giận dữ' },
      { eng: 'animal', vie: 'động vật, thú vật' },
      { eng: 'answer,vie: trả lời' },
      { eng: 'any',vie: 'bất cứ,một chút nào, tí nào' },
      { eng: 'appear',vie: 'xuất hiện, hiện ra, trình diện' },
      { eng: 'apple',vie: 'quả táo' },
      { eng: 'are', vie: 'thì ,là'},
      { eng: 'area', vie: 'diện tích, bề mặt' },
      { eng: 'arm',vie: 'vũ trang, trang bị'},
      { eng: 'arrange',vie: 'sắp xếp, sắp đặt, sửa soạn' },
      { eng: 'arrive',vie: 'đến, tới nơi' },
      { eng: 'art',vie: 'nghệ thuật, mỹ thuật' },
      { eng: 'as',vie:'như'},
      { eng: 'ask',vie: 'hỏi' },
      { eng: 'at',vie: 'ở tại vị trí'},
      { eng: 'atom',vie: 'nguyên tử' },
      { eng: 'baby', vie: 'trẻ thơ' },
      { eng: 'back', vie: 'lưng, về phía sau, trở lại' },
      { eng: 'bad', vie: 'xấu, tồi' },
      { eng: 'ball', vie: 'quả bóng' },
      { eng: 'band', vie: 'băng, đai, nẹp' },
      { eng: 'bank', vie: 'ngân hàng'},
      { eng: 'bar', vie: 'quán bán rượu' },
      { eng: 'base', vie: 'đặt tên, đặt cơ sở trên cái gì' },
      { eng: 'basic', vie: 'cơ bản, cơ sở' },
      { eng: 'bat', vie: 'vợt'},
      { eng: 'be', vie: 'thì, là'},
      { eng: 'bear', vie: 'mang, cầm, vác, đeo, ôm' },
      { eng: 'beat', vie: 'đánh đập, đấm' },
      { eng: 'beauty', vie: 'người đẹp' },
      { eng: 'bed', vie: 'cái giường' },
      { eng: 'been', vie: 'thì, là' },
      { eng: 'before', vie: 'trước, đằng trước' },
      { eng: 'began', vie: 'bắt đầu, mở đầu, khởi đầu' },
      { eng: 'begin', vie: 'bắt đầu, khởi đầu' },
      { eng: 'behind', vie: 'sau, ở đằng sau' },
      { eng: 'believe', vie: 'tin, tin tưởng' },
      { eng: 'bell', vie: 'cái chuông, tiếng chuông' },
      { eng: 'best',  vie: 'tốt nhất' },
      { eng: 'better', vie: 'tốt hơn' },
      { eng: 'between', vie: 'giữa, ở giữa' },
      { eng: 'big', vie: 'to, lớn' },
      { eng: 'bird', vie: 'chim' },
      { eng: 'bit', vie: 'miếng, mảnh' },
      { eng: 'black', vie: 'màu đen' },
      { eng: 'block', vie: 'khối, tảng  làm ngăn cản, ngăn chặn' },
      { eng: 'blood', vie: 'sự tàn sát, chem giết' },
      { eng: 'blow', vie: 'sự nở hoa' },
      { eng: 'blue', vie: 'xanh, màu xanh' },
      { eng: 'board', vie: 'lát ván, lót ván' },
      { eng: 'boat', vie: 'tàu, thuyền' },
      { eng: 'body', vie: 'thân thể, thân xác' },
      { eng: 'bone', vie: 'xương' },
      { eng: 'book', vie: 'sách' },
      { eng: 'born', vie: 'sinh' },
      { eng: 'both', vie: 'cả hai'},
      { eng: 'bottom', vie: 'cuối, cuối cùng' },
      { eng: 'bought', vie: 'mua' },
      { eng: 'box', vie: 'hộp, thùng' },
      { eng: 'boy', vie: 'con trai, thiếu niên' },
      { eng: 'branch', vie: 'nhành cây, nhánh song, ngả đường' },
      { eng: 'bread', vie: 'bánh mỳ' },
      { eng: 'break', vie: 'sự gãy, sự vỡ' },
      { eng: 'bright', vie: 'sáng, sáng chói' },
      { eng: 'bring', vie: 'mang, cầm, xách lại' },
      { eng: 'broad', vie: 'rộng' },
      { eng: 'broke', vie: 'khánh kiệt, túng quẫn, bần cùng' },
      { eng: 'brother', vie: 'anh, em trai' },
      { eng: 'brought', vie: 'đưa lại' },
      { eng: 'brown', vie: 'nâu, màu nâu' },
      { eng: 'build', vie: 'xây dựng' },
      { eng: 'burn', vie: 'đốt, đốt cháy, thắp, nung, thiêu' },
      { eng: 'busy', vie: 'bận, bận rộn' },
      { eng: 'but', vie: 'nhưng' },
      { eng: 'buy', vie: 'mua' },
      { eng: 'by', vie: 'bởi, bằng' },
      { eng: 'call', vie: 'tiếng kêu, tiếng gọi' },
      { eng: 'came', vie: 'khung chì lắp kinh' },
      { eng: 'camp', vie: 'cắm trại, hạ trại' },
      { eng: 'can', vie: 'có thể' },
      { eng: 'capital', vie: 'thủ đô'},
      { eng: 'captain', vie: 'thủ lĩnh' },
      { eng: 'car', vie: 'xe hơi' },
      { eng: 'card', vie: 'thẻ, thiếp' },
      { eng: 'care', vie: 'chăm sóc' },
      { eng: 'carry', vie: 'mang, vác, khuân chở' },
      { eng: 'case', vie: 'vỏ, ngăn, túi' },
      { eng: 'cat', vie: 'con mèo' },
      { eng: 'catch', vie: 'bắt lấy, nắm lấy, tóm lấy, chộp lấy' },
      { eng: 'caught', vie: 'cái bắt, cái vồ, cái chộp' },
      { eng: 'cause', vie: 'gây ra, gây nên' },
      { eng: 'cell', vie: 'ô, ngăn' },
      { eng: 'cent', vie: 'đồng xu'},
      { eng: 'center', vie: 'trung tâm'},
      { eng: 'century', vie: 'thế kỷ'},
      { eng: 'certain', vie: 'chắc chắn' },
      { eng: 'chair', vie: 'ghế' },
      { eng: 'chance', vie: 'sự may mắn' },
      { eng: 'change', vie: 'thay đổi, sự thay đổi, sự biến đổi' },
      { eng: 'character', vie: 'tính cách, đặc tính, nhân vật' },
      { eng: 'charge', vie: 'giao việc' },
      { eng: 'chart', vie: 'vẽ đồ thị, lập biểu đồ' },
      { eng: 'check', vie: 'sự kiểm tra' },
      { eng: 'chick', vie: 'gà,chim con' },
      { eng: 'chief', vie: 'người đứng đầu, xếp' },
      { eng: 'child', vie: 'đứa bé, đứa trẻ' },
      { eng: 'children', vie: 'đứa bé, đứa trẻ' },
      { eng: 'choose', vie: 'chọn, lựa chọn' },
      { eng: 'chord', vie: 'dây hạc' },
      { eng: 'circle', vie: 'đường tròn, hình tròn' },
      { eng: 'city', vie: 'thành phố'},
      { eng: 'claim', vie: 'sự thỉnh cầu' },
      { eng: 'class', vie: 'lớp học' },
      { eng: 'clean', vie: 'sạch, sạch sẽ' },
      { eng: 'clear', vie: 'lau chùi, quét dọn' },
      { eng: 'climb', vie: 'leo, trèo' },
      { eng: 'clock', vie: 'đồng hồ' },
      { eng: 'close', vie: 'đóng'},
      { eng: 'clothe', vie: 'mặc quần áo cho' },
      { eng: 'cloud', vie: 'mây, đám mây' },
      { eng: 'coast', vie: 'bờ biển' },
      { eng: 'coat', vie: 'áo choàng' },
      { eng: 'cold', vie: 'lạnh, sự lạnh lẽo, lạnh nhạt' },
      { eng: 'collect', vie: 'sưu tập, tập trung lại' },
      { eng: 'colony', vie: 'thuộc địa' },
      { eng: 'color',  vie: 'màu sắc'},
      { eng: 'column', vie: 'cột, mục' },
      { eng: 'come', vie: 'đến, tới, đi đến, đi tới' },
      { eng: 'common', vie: 'công, phổ biến'},
      { eng: 'company', vie: 'công ty'},
      { eng: 'compare', vie: 'so sánh, đối chiếu' },
      { eng: 'complete', vie: 'hoàn thành, xong' },
      { eng: 'condition', vie: 'điều kiện, tình cảnh, tình thế' },
      { eng: 'connect', vie: 'kết nối, nối' },
      { eng: 'consider', vie: 'để ý, quan tâm, lưu ý đến' },
      { eng: 'consonant', vie: 'thuận tai, êm ái, du dương' },
      { eng: 'contain', vie: 'bao hàm, chứa đựng, bao gồm' },
      { eng: 'continent', vie: 'lục địa, đại lục địa Bắc Mỹ' },
      { eng: 'continue', vie: 'tiếp tục, làm tiếp' },
      { eng: 'control', vie: 'sự điều khiển, quyền hành, quyền lực, quyền chỉ huy' },
      { eng: 'cook', vie: 'nấu ăn, người nấu ăn' },
      { eng: 'cool', vie: 'làm mát' },
      { eng: 'copy', vie: 'sao chép, bắt chước' },
      { eng: 'corn', vie: 'chai'},
      { eng: 'corner', vie: 'nhà, phố'},
      { eng: 'correct', vie: 'sửa, sửa chữa' },
      { eng: 'cost', vie: 'trả giá, phải trả' },
      { eng: 'cotton', vie: 'bông, chỉ, sợi' },
      { eng: 'could', vie: 'có thể,có khả năng' },
      { eng: 'count', vie: 'đếm, tính' },
      { eng: 'country', vie: 'nước, quốc gia, đất nước' },
      { eng: 'course', vie: 'tiến trình, quá trình diễn tiến'},
      { eng: 'cover', vie: 'vỏ, vỏ bọc' },
      { eng: 'cow', vie: 'con bò'},
      { eng: 'crease', vie: 'nếp nhăn, nếp gấp' },
      { eng: 'create', vie: 'sáng tạo, tạo nên' },
      { eng: 'crop', vie: 'vụ mùa' },
      { eng: 'cross', vie: 'băng qua, vượt qua' },
      { eng: 'crowd', vie: 'đám đông' },
      { eng: 'cry', vie: 'sự khóc, tiếng khóc, sự kêu la' },
      { eng: 'current', vie: 'hiện hành, phổ biến, hiện nay'},
      { eng: 'cut', vie: 'sự cắt' },
      { eng: 'dad', vie: 'bố, cha' },
      { eng: 'dance', vie: 'sự nhảy múa,khiêu vũ' },
      { eng: 'danger', vie: 'nguy cơ, mối đe dọa' },
      { eng: 'dark', vie: 'bóng tối, ám muội' },
      { eng: 'day', vie: 'ngày, ban ngày' },
      { eng: 'dead', vie: 'chết, tắt' },
      { eng: 'deal', vie: 'sự giao dịch, thỏa thuận mua bán' },
      { eng: 'dear', vie: 'kính thưa, thưa' },
      { eng: 'death', vie: 'sự chết, cái chết' },
      { eng: 'decide', vie: 'quyết định, giải quyết, phân xử' },
      { eng: 'decimal', vie: 'thập phân' },
      { eng: 'deep', vie: 'sâu, khó lường, bí ẩn' },
      { eng: 'degree', vie: 'độ' },
      { eng: 'depend', vie: 'phụ thuộc, tuỳ thuộc, tuỳ thuộc' },
      { eng: 'describe', vie: 'diễn tả, miêu tả, mô tả' },
      { eng: 'desert', vie: 'rời bỏ, bỏ trốn' },
      { eng: 'design', vie: 'thiết kế' },
      { eng: 'determine', vie: 'quyết định' },
      { eng: 'develop', vie: 'trình bày, bày tỏ' },
      { eng: 'dictionary', vie: 'từ điển' },
      { eng: 'die', vie: 'chết, từ trần, hy sinh' },
      { eng: 'differ', vie: 'khác, không giống' },
      { eng: 'difficult', vie: 'khó, khó khăn, gay go' },
      { eng: 'direct', vie: 'gửi, viết cho ai, điều khiển' },
      { eng: 'discuss', vie: 'thảo luận, tranh luận' },
      { eng: 'distant', vie: 'xa, cách, xa cách' },
      { eng: 'divide', vie: 'chia, chia ra, phân ra' },
      { eng: 'division', vie: 'sự chia, sự phân chia, sự phân loại' },
      { eng: 'do', vie: 'làm' },
      { eng: 'doctor', vie: 'bác sĩ y khoa, tiến sĩ' },
      { eng: 'does', vie: 'nai cái' },
      { eng: 'dog', vie: 'con chó' },
      { eng: 'dollar', vie: 'đô la Mỹ' },
      { eng: 'done', vie: 'xong, hoàn thành, đã thực hiện' },
      { eng: 'door', vie: 'cửa, cửa ra vào' },
      { eng: 'double', vie: 'làm gấp đôi' },
      { eng: 'down', vie: 'xuống' },
      { eng: 'draw', vie: 'vẽ, kéo' },
      { eng: 'dream', vie: 'giấc mơ, mơ' },
      { eng: 'dress', vie: 'ăn mặc' },
      { eng: 'drink', vie: 'uống' },
      { eng: 'drive', vie: 'cuộc đua xe khiển' },
      { eng: 'drop', vie: 'rơi' },
      { eng: 'dry', vie: 'làm khô, sấy khô' },
      { eng: 'duck', vie: 'con vịt, vịt cái' },
      { eng: 'during', vie: 'trong lúc, trong thời gian' },
      { eng: 'each', vie: 'mỗi' },
      { eng: 'ear', vie: 'tai' },
      { eng: 'early', vie: 'sớm' },
      { eng: 'earth', vie: 'đất, trái đất' },
      { eng: 'ease', vie: 'làm dễ chịu' },
      { eng: 'east', vie: 'hướng đông, phía đông, đông, ở phía đông' },
      { eng: 'eat', vie: 'ăn' },
      { eng: 'edge', vie: 'lưỡi, cạnh sắc' },
      { eng: 'effect', vie: 'hiệu ứng, hiệu quả, kết quả' },
      { eng: 'egg', vie: 'trứng' },
      { eng: 'eight', vie: 'tám' },
      { eng: 'either', vie: 'cũng phải thế' },
      { eng: 'electric', vie: 'điện, có điện, phát điện' },
      { eng: 'element', vie: 'nguyên tố'},
      { eng: 'else', vie: 'nếu không' },
      { eng: 'end', vie: 'kết thúc, chấm dứt' },
      { eng: 'enemy', vie: 'kẻ thù, quân địch' },
      { eng: 'energy', vie: 'năng lượng, nghị lực, sinh lực' },
      { eng: 'engine', vie: 'máy, động cơ'},
      { eng: 'enough', vie:  'đủ'},
      { eng: 'enter', vie: 'đi vào, gia nhập' },
      { eng: 'equal', vie: 'bằng, ngang' },
      { eng: 'equate', vie: 'làm cân bằng, san bằng' },
      { eng: 'especially', vie: 'đặc biệt là, nhất là' },
      { eng: 'even', vie: 'bằng phẳng, điềm đạm, ngang bằng' },
      { eng: 'evening', vie: 'buổi chiều, tối' },
      { eng: 'event', vie: 'sự việc, sự kiện' },
      { eng: 'ever', vie: 'từng, từ trước tới giờ' },
      { eng: 'every', vie: 'mỗi, mọi' },
      { eng: 'exact', vie: 'chính xác, đúng' },
      { eng: 'example', vie: 'thí dụ, ví dụ' },
      { eng: 'except', vie: 'trừ phi' },
      { eng: 'excite', vie: 'kích thích, kích động' },
      { eng: 'exercise', vie: 'làm, thi hành, thực hiện' },
      { eng: 'expect', vie: 'liệu trước' },
      { eng: 'experience', vie: 'trải qua, nếm mùi' },
      { eng: 'experiment', vie: 'thí nghiệm' },
      { eng: 'eye', vie: 'mắt' },
      { eng: 'face', vie: 'đương đầu, đối phó, đối mặt' },
      { eng: 'fact', vie: 'việc, sự việc, sự kiện' },
      { eng: 'fair', vie: 'thuận lợi' },
      { eng: 'fall', vie: 'rơi, ngã, sự rơi, ngã' },
      { eng: 'family', vie: 'gia đình, thuộc gia đình' },
      { eng: 'famous', vie: 'nổi tiếng' },
      { eng: 'far', vie: 'xa' },
      { eng: 'farm', vie: 'trang trại' },
      { eng: 'fast', vie: 'nhanh' },
      { eng: 'fat', vie: 'mỡ, chất béo' },
      { eng: 'father', vie: 'cha'}, 
      { eng: 'favor', vie: 'sự quý mến' },
      { eng: 'fear', vie: 'sợ, lo ngại' },
      { eng: 'feed', vie: 'cho ăn, nuôi' },
      { eng: 'feel', vie: 'cảm thấy' },
      { eng: 'feet', vie: 'chân, bàn chân' },
      { eng: 'fell', vie: 'da lông thú vật'},
      { eng: 'felt', vie: 'nỉ, phớt' },
      { eng: 'few', vie: 'một ít, một vài' },
      { eng: 'field', vie: 'cánh đồng, bãi chiến trường' },
      { eng: 'fig', vie: 'quả vả' },
      { eng: 'fight', vie: 'sự đấu tranh, cuộc chiến đấu' },
      { eng: 'figure', vie: 'hình dung, miêu tả' },
      { eng: 'fill', vie: 'làm đấy, lấp kín' },
      { eng: 'final', vie: 'cuối cùng, cuộc đấu chung kết' },
      { eng: 'find', vie: 'tìm, tìm thấy' },
      { eng: 'fine', vie: 'tốt, giỏi' },
      { eng: 'finger', vie: 'ngón tay' },
      { eng: 'finish', vie: 'sự kết thúc, phần cuối' },
      { eng: 'fire', vie: 'đốt cháy' },
      { eng: 'first', vie: 'người, vật đầu tiên, thứ nhất' },
      { eng: 'fish', vie: 'câu cá, bắt cá' },
      { eng: 'fit', vie: 'thích hợp, xứng đáng' },
      { eng: 'five', vie: 'năm' },
      { eng: 'flat', vie: 'dãy phòng, căn phòng, mặt phẳng' },
      { eng: 'floor', vie: 'sàn, tầng' },
      { eng: 'flow', vie: 'chảy' },
      { eng: 'flower', vie: 'hoa, bông, đóa, cây hoa' },
      { eng: 'fly', vie: 'sự bay, quãng đường bay' },
      { eng: 'follow', vie: 'đi theo sau, theo, tiếp theo' },
      { eng: 'food', vie: 'đồ ăn, thức, món ăn' },
      { eng: 'foot', vie: 'chân, bàn chân' },
      { eng: 'for', vie: 'cho, dành cho'},
      { eng: 'force', vie: 'ép buộc, cưỡng ép' },
      { eng: 'forest', vie: 'rừng' },
      { eng: 'form', vie: 'làm thành, được tạo thành' },
      { eng: 'forward', vie: 'ở phía trước, tiến về phía trước' },
      { eng: 'found', vie: 'tìm, tìm thấy' },
      { eng: 'four', vie: 'bốn' },
      { eng: 'fraction', vie: 'phân số' },
      { eng: 'free', vie: 'miễn phí, tự do'},
      { eng: 'fresh', vie: 'tươi, tươi tắn' },
      { eng: 'friend', vie: 'người bạn' },
      { eng: 'from', vie: 'từ' },
      { eng: 'front', vie: 'đằng trước, về phía trước' },
      { eng: 'fruit', vie: 'quả, trái cây' },
      { eng: 'full', vie: 'đầy, đầy đủ' },
      { eng: 'fun', vie: 'hài hước' },
      { eng: 'game', vie: 'trò chơi' },
      { eng: 'garden', vie: 'vườn' },
      { eng: 'gas', vie: 'khí, hơi đốt' },
      { eng: 'gather', vie: 'hái, lượm, thu thập' },
      { eng: 'gave', vie: 'cho, biếu, tặng, ban' },
      { eng: 'general', vie: 'chung, tổng' },
      { eng: 'gentle', vie: 'hiền lành, dịu dàng, nhẹ nhàng' },
      { eng: 'get', vie: 'được, có được' },
      { eng: 'girl', vie: 'con gái' },
      { eng: 'give', vie: 'cho, biếu, tặng' },
      { eng: 'glad', vie: 'vui lòng, sung sướng' },
      { eng: 'glass', vie: 'kính, thủy tinh, cái cốc, ly' },
      { eng: 'go', vie: 'đi' },
      { eng: 'gold', vie: 'bằng vàng' },
      { eng: 'gone', vie: 'đã trôi qua, đã qua' },
      { eng: 'good', vie: 'điều tốt, điều thiện' },
      { eng: 'got', vie: 'lấy được' },
      { eng: 'govern', vie: 'cầm quyền, cai trị' },
      { eng: 'grand', vie: 'rộng lớn, vĩ đại' },
      { eng: 'grass', vie: 'bãi cỏ, đồng cỏ' },
      { eng: 'gray', vie: 'xám, hoa râm' },
      { eng: 'great', vie: 'to, lớn, vĩ đại' },
      { eng: 'green', vie: 'xanh lá cây' },
      { eng: 'grew', vie: 'mọc mầm, đâm chồi nẩy nở' },
      { eng: 'ground', vie: 'mặt đất, đất, bãi đất' },
      { eng: 'group', vie: 'nhóm' },
      { eng: 'grow', vie: 'mọc, mọc lên' },
      { eng: 'guess', vie: 'sự đoán, sự ước chừng' },
      { eng: 'guide', vie: 'dẫn đường, chỉ đường' },
      { eng: 'gun', vie: 'súng' },
      { eng: 'had', vie: 'có' },
      { eng: 'hair', vie: 'tóc' },
      { eng: 'half', vie: 'một nửa' },
      { eng: 'hand', vie: 'trao tay, truyền cho' },
      { eng: 'happen', vie: 'xảy ra, xảy đến' },
      { eng: 'happy', vie: 'hạnh phúc' },
      { eng: 'hard', vie: 'hết sức cố gắng, tích cực' },
      { eng: 'has', vie: 'có' },
      { eng: 'hat', vie: 'cái mũ' },
      { eng: 'have', vie: 'có' },
      { eng: 'he', vie: 'anh ấy, ông ấy' },
      { eng: 'head', vie: 'chỉ huy, lãnh đại, dẫn đầu' },
      { eng: 'hear', vie: 'nghe' },
      { eng: 'heard', vie: 'nghe' },
      { eng: 'heart', vie: 'tim, trái tim' },
      { eng: 'heat', vie: 'hơi nóng, sức nóng' },
      { eng: 'heavy', vie: 'nặng, nặng nề' },
      { eng: 'held', vie: 'khoang tàu thuỷ' },
      { eng: 'help', vie: 'sự giúp đỡ'},
      { eng: 'her', vie: 'nó, chị ấy, cô ấy, bà ấy' },
      { eng: 'here', vie: 'đây, ở đây' },
      { eng: 'high', vie: 'cao, ở mức độ cao' },
      { eng: 'hill', vie: 'đồi' },
      { eng: 'him', vie: 'hắn, ông ấy, anh ấy' },
      { eng: 'his', vie: 'anh ấy' },
      { eng: 'history', vie: 'lịch sử, sử học' },
      { eng: 'hit', vie: 'đòn, cú đấm' },
      { eng: 'hold', vie: 'sự cầm, sự nắm giữ' },
      { eng: 'hole', vie: 'hang' },
      { eng: 'home', vie: 'ở tại nhà, nước mình' },
      { eng: 'hope', vie: 'nguồn hy vọng' },
      { eng: 'horse', vie: 'ngựa' },
      { eng: 'hot', vie: 'nóng,nóng bức' },
      { eng: 'hour', vie: 'giờ' },
      { eng: 'house', vie: 'nhà, căn nhà, toàn nhà' },
      { eng: 'how', vie: 'thế nào, như thế nào, làm sao, ra sao' },
      { eng: 'huge', vie: 'to lớn, khổng lồ' },
      { eng: 'human', vie: 'con người, loài người' },
      { eng: 'hundred', vie: 'trăm' },
      { eng: 'hunt', vie: 'săn, đi săn' },
      { eng: 'hurry', vie: 'sự vội vàng, sự gấp rút' },
      { eng: 'ice', vie: 'băng, nước đá' },
      { eng: 'idea', vie: 'ý tưởng, quan niệm' },
      { eng: 'if', vie: 'nếu, nếu như' },
      { eng: 'imagine', vie: 'tưởng rằng, cho rằng' },
      { eng: 'in', vie: 'vào' },
      { eng: 'inch', vie: 'đo chiều dài Anh'},
      { eng: 'include', vie: 'bao gồm, tính cả'},
      { eng: 'indicate', vie: 'biểu thị, trình bày ngắn gọn' },
      { eng: 'industry', vie: 'công nghiệp, kỹ nghệ' },
      { eng: 'insect', vie: 'sâu bọ, côn trùng' },
      { eng: 'instant', vie: 'lúc, chốc lát' },
      { eng: 'instrument', vie: 'dụng cụ âm nhạc khí' },
      { eng: 'interest', vie: 'làm chú ý' },
      { eng: 'invent', vie: 'phát minh, sáng chế' },
      { eng: 'iron', vie: 'bọc sắt' },
      { eng: 'island', vie: 'hòn đảo' },
      { eng: 'it', vie: 'cái đó, điều đó, con vật đó' },
      { eng: 'job', vie: 'việc, việc làm' },
      { eng: 'join', vie: 'nối, chắp, ghép' },
      { eng: 'joy', vie: 'niềm vui, sự vui mừng' },
      { eng: 'jump', vie: 'sự nhảy, bước nhảy' },
      { eng: 'just', vie: 'vừa mới, chỉ' },
      { eng: 'keep', vie: 'giữ, giữ lại' },
      { eng: 'kept', vie: 'giữ, giữ lại' },
      { eng: 'key', vie: 'khóa'},
      { eng: 'kill', vie: 'giết, tiêu diệt' },
      { eng: 'kind', vie: 'tử tế, có lòng tốt' },
      { eng: 'king', vie: 'vua, quốc vương' },
      { eng: 'knew', vie: 'hiểu biết' },
      { eng: 'know', vie: 'biết' },
      { eng: 'lady', vie: 'quý bà, tiểu thư' },
      { eng: 'lake', vie: 'hồ' },
      { eng: 'land', vie: 'đất canh tác, đất đai' },
      { eng: 'language', vie: 'ngôn ngữ' },
      { eng: 'large', vie: 'rộng, lớn, to' },
      { eng: 'last', vie: 'cuối cùng'},
      { eng: 'late', vie: 'trễ, muộn' },
      { eng: 'laugh', vie: 'tiếng cười' },
      { eng: 'law', vie: 'luật' },
      { eng: 'lay', vie: 'xếp, đặt, bố trí' },
      { eng: 'lead', vie: 'sự lãnh đạo, sự hướng dẫn' },
      { eng: 'learn', vie: 'học, nghiên cứu' },
      { eng: 'least', vie: 'ít nhất' },
      { eng: 'leave', vie: 'bỏ đi, rời đi, để lại' },
      { eng: 'left', vie: 'về phía trái' },
      { eng: 'leg', vie: 'chân'},
      { eng: 'length', vie: 'chiều dài, độ dài' },
      { eng: 'less', vie: 'số lượng ít hơn' },
      { eng: 'let', vie: 'cho phép, để cho' },
      { eng: 'letter', vie: 'chữ cái, mẫu tự' },
      { eng: 'level', vie: 'bằng, ngang bằng' },
      { eng: 'lie', vie: 'lời nói dối, sự dối trá' },
      { eng: 'life', vie: 'đời, sự sống' },
      { eng: 'lift', vie: 'sự nâng, sự nhấc lên' },
      { eng: 'light', vie: 'thắp sáng' },
      { eng: 'like', vie: 'thích, như' },
      { eng: 'line', vie: 'dây, đường, tuyến' },
      { eng: 'liquid', vie: 'lỏng, êm ái, du dương, không vững' },
      { eng: 'list', vie: 'ghi vào danh sách' },
      { eng: 'listen', vie: 'nghe, lắng nghe' },
      { eng: 'little', vie: 'một chút' },
      { eng: 'live', vie: 'sống' },
      { eng: 'locate', vie: 'xác định vị trí, định vị' },
      { eng: 'log', vie: 'khúc gỗ mới đốn, khúc gỗ mới xẻ' },
      { eng: 'lone', vie: 'hiu quạnh' },
      { eng: 'long', vie: 'lâu' },
      { eng: 'look', vie: 'cái nhìn' },
      { eng: 'lost', vie: 'thua, mất' },
      { eng: 'lot', vie: 'sự chọn bằng cách rút thăm' },
      { eng: 'loud', vie: 'to, lớn' },
      { eng: 'love', vie: 'yêu, thích' },
      { eng: 'low', vie: 'thấp, bé, lùn' },
      { eng: 'machine', vie: 'máy, máy móc' },
      { eng: 'made', vie: 'thực hiện' },
      { eng: 'magnet', vie: 'nam châm' },
      { eng: 'main', vie: 'chính, chủ yếu, trọng yếu nhất' },
      { eng: 'major', vie: 'lớn, nhiều hơn, trọng đại, chủ yếu' },
      { eng: 'make', vie: 'sự chế tạo' },
      { eng: 'man', vie: 'đàn ông' },
      { eng: 'many', vie: 'nhiều' },
      { eng: 'map', vie: 'bản đồ' },
      { eng: 'mark', vie: 'đánh dấu, ghi dấu' },
      { eng: 'market', vie: 'chợ, thị trường' },
      { eng: 'mass', vie: 'quần chúng, đại chúng' },
      { eng: 'master', vie: 'chủ, chủ nhân, thầy giáo, thạc sĩ' },
      { eng: 'match', vie: 'trận thi đấu, đối thủ'},
      { eng: 'material', vie: 'hữu hình' },
      { eng: 'matter', vie: 'có ý nghĩa, có tính chất quan trọng' },
      { eng: 'may', vie: 'tháng 5' },
      { eng: 'me', vie: 'tôi, tao, tớ'},
      { eng: 'mean', vie: 'nghĩa, có nghĩa là' },
      { eng: 'meant', vie: 'trung gian, trung dung' },
      { eng: 'measure', vie: 'đơn vị đo lường' },
      { eng: 'meat', vie: 'thịt' },
      { eng: 'meet', vie: 'gặp, gặp gỡ' },
      { eng: 'melody', vie: 'giai điệu' },
      { eng: 'men', vie: 'người,con người' },
      { eng: 'metal', vie: 'kim loại' },
      { eng: 'method', vie: 'phương pháp, cách thức' },
      { eng: 'middle', vie: 'giữa, ở giữa' },
      { eng: 'might',  vie: 'may có thể, có lẽ' },
      { eng: 'mile', vie: 'dặm đường'},
      { eng: 'milk', vie: 'sữa' },
      { eng: 'million', vie: 'triệu' },
      ]
      }
      },
  methods: {
    search: function(){
      var x;
      for(x of this.word){
        if(x.eng == this.text){
          var i = this.word.indexOf(x);
          return this.target = this.word[i].vie;
        }
      }
      return null;
    },
    add: function(){
      var x = {eng: this.english, vie: this.vietnamese};
      this.word.push(x);
    },
    deleteWord: function(){
      var x;
      for(x of this.word){
        if(x.eng == this.english){
          var key = this.word.indexOf(x);
          console.log(key);
          this.word.splice(key, 1);
        }
      }
    },
    history: function(){
      var x={eng: this.text, vie: this.search()};
      if(this.text != null && this.search() != null){
        this.his.push(x);
      }
    }
  }
}
</script>

<style>
  *{
    color: #2196F3;
    font-family: consolas;
  }
  #app{
    border: 1px solid #ebebeb;
    margin: 50px auto;
    background-color: #031321;
    width: 500px;
    height: 500px;
  }
  .wrapper > button{
    display: block;
    padding: 10px 5px;
    margin: 20px auto;
  }
button {
  position: relative;
  display: inline-block;
  padding: 5px 10px;
  color: #2196F3;
  text-transform: uppercase;
  letter-spacing: 1px;
  text-decoration: none;
  font-size: 20px;
  overflow: hidden;
  transition: 0.2s;
  background-color: #031321;
  border: none;
  -webkit-box-reflect: below 1px linear-gradient(transparent, #0005);
}
button:nth-child(4n+1){
  filter: hue-rotate(300deg);
}
button:nth-child(4n+2){
  filter: hue-rotate(400deg);
}
button:nth-child(4n+3){
  filter: hue-rotate(490deg);
}
button:nth-child(4n+4){
  filter: hue-rotate(550deg);
}
button:hover{
  color:#255784;
  background-color: #2196f3;
  box-shadow: 0 0 10px #2196f3, 0 0 40px #2196f3, 0 0 80px #2196f3;
  transition-delay: 0.3s;
  border: none;
}
button span{
  position: absolute;
  display: block;
}
button > span:nth-child(1){
  top: 0;
  left: -100%;
  width: 100%;
  height: 2px;
  background: linear-gradient(90deg,transparent,#2196f3);
  animation: animate1 1s linear infinite;
}
@keyframes animate1{
  0%{
    left: -100%;
  }
  50%,100%{
    left: 100%;
  }
}
button > span:nth-child(3){
  bottom: 0;
  right: -100%;
  width: 100%;
  height: 2px;
  background: linear-gradient(270deg,transparent,#2196f3);
  animation: animate3 1s linear infinite;
  animation-delay: 0.5s;
}
@keyframes animate3{
  0%{
    right: -100%;
  }
  50%,100%{
    right: 100%;
  }
}
button > span:nth-child(2){
  top: -100%;
  right: 0;
  width: 2px;
  height: 100%;
  background: linear-gradient(180deg,transparent,#2196f3);
  animation: animate2 1s linear infinite;
  animation-delay: 0.25s;
}
@keyframes animate2{
  0%{
    top: -100%;
  }
  50%,100%{
    top: 100%;
  }
}
button > span:nth-child(4){
  bottom: -100%;
  left: 0;
  width: 2px;
  height: 100%;
  background: linear-gradient(360deg,transparent,#2196f3);
  animation: animate4 1s linear infinite;
  animation-delay: 0.75s;
}
@keyframes animate4{
  0%{
    bottom: -100%;
  }
  50%,100%{
    bottom: 100%;
  }
}
input{
  padding: 5px 10px;
  outline: none;
  border-radius: 20px;
  border: none;
}
input:hover{
  box-shadow: 0 0 10px #2196f3, 0 0 40px #2196f3, 0 0 80px #2196f3;
}
.diction{
  margin-top: 150px;
}
.diction > button{
  display: block;
  margin: 20px auto;
}
.uses {
  margin-top: 150px;
}
.uses > button{
  display: block;
  margin: 20px auto;
}
.search > #load{
  margin-top: 100px;
}
.search > #load > input{
  display: block;
  margin: 20px auto;
  width: 250px;
  height: 30px;
}
.search > #load > button{
  display: block;
  margin: 20px auto;
}
.search > #processing{
  margin-top: 100px;
}
.search > #processing span{
  display: block;
  margin: 0 auto;
  text-align: center;
}
.search > #processing button{
  display: block;
  margin:20px auto;
}
.search > #processing input{
  display: block;
  margin: 20px auto;
  height: 30px;
}
.deleteW{
  margin: 150px;
}
.deleteW > div > span{
  display: block;
  margin: 20px auto;
  text-align: center;
}
.deleteW > div > input{
  display: block;
  margin: 0px auto;
  height: 30px;
}
.deleteW > div > button{
  display: block;
  margin:20px auto;
}
.deleteW > button{
  display: block;
  margin:20px auto;
}
.wordhis > button{
  display: block;
  margin: 20px auto;
}
.wordhis > #hisSub{
  text-align: center;
}

</style>
