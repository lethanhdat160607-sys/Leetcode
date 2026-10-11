#Code C

#Two-pass Hash Table


```
//Macro là gì ? 
//Macron giống như một bản bản thay thế trước khi biên dịch code trình biên dịch C sẻ quét tự động thay 
int* twoSum(int* nums, int numsSize, int target, int* returnSize) {
    struct hashTable {
        int key; // lưu giá trị cần tìm 
        int value; // lưu chỉ số 
        UT_hash_handle hh; // Macro bắt buộc của thư viện quản lý 
    } *hashTable = NULL, *item, *tmpItem; // khai báo con trỏ (quy tắt syntax)
   
    /* viet tat của dang hashTable trên
    struct hashTable *hashTable = NULL; // Khai báo con trỏ gốc và gán bằng NULL
    struct hashTable *item;             // Khai báo con trỏ item tạm
    struct hashTable *tmpItem;          // Khai báo con trỏ tmpItem tạm
    */
    
    for (int i = 0; i < numsSize; i++) { // duyệt các phần tử
        // HASH_FINT_INT là một Macor tiền sử lý 
        // hashTable dây là con trỏ gốc của mảng băm 
        // nums[i] dò tìm phần tử thứ i của mảng nums và dầu & là lấy địa chỉ của ô nhớ. thư viên uthash cần địa chỉ này để so sanh giá trị bên trong với key 
        // item đây là biến con tror nhận kết quả trả về sau khi chạy sẻ biến nó đi hướng nào 
        HASH_FIND_INT(hashTable, &nums[i], item); 
        
        // nếu item có tồn tại 
        if (item) { // if (item != NULL)
            int* result = malloc(sizeof(int) * 2); // cấp phát bộ nhớ cho mảng nhân 2 vì để bài kết quả luôn ra chỉ ra 2 chỉ số 
            result[0] = item->value; // lưu chỉ số của con số đầu tiền của mảng kết quả và item con trỏ đến ô dữ liệu value 
            result[1] = i; // lưu chỉ số của con số hiện tại vào ô nhớ thứ 2 gán biến i ở vong lặp
            *returnSize = 2; // thông báo ra kết quả đúng 2 phần tử
            //một Macro vòng lặp của thư viện uthash dùng để duyệt qua từng phần tử có trong mảng băm 
            //hh cấu trúc đã khai báo lúc đầu, 
            //hashTable bảng băm gốc chứa danh sách các phần tử
            //item biến đại diện cho phần tử hiện tại đang chuẩn bị xử lí, tmpItem đây là biến tạm.
            /*Tại sao lại cần tmpItem? vì vừa xóa mất cái item khoỉ bảng băm máy tính sẽ mất dấu không biết phần tử tiếp theo nằm ở đâu nữa. 
            Do đó, trước khi xóa, macro này dùng tmpItem để ghi nhớ lại địa chỉ của phần tử tiếp theo nhờ vậy mới tiếp lặp đến hết mảng 
            1*/ 
            HASH_ITER(hh, hashTable, item, tmpItem) {
                HASH_DEL(hashTable, item); // gỡ bỏ phần tử item hiện tại ra khỏi bẳng băm hashTable 
                free(item); // giải phòng bộ nhớ item
            }

            return result; // trả về giá trị result là 2 chỉ số  mà ta đã lưu 
        }
        item = malloc(sizeof(struct hashTable)); // Tạo 3 ô: key, value, hh tạo bộ nhớ Heap
        item->key = target - nums[i]; // tính toán và lưu phần bù ( target - nums[i]) vào ô key 
        item->value = i; // lưu chỉ số i hiện tại
        HASH_ADD_INT(hashTable, key, item);// đây là macro của thư viện uthash dùng để nhét cái item vừa tạo vào bảng băm hashTable, lấy trường key làm chìa khóa tra cứu
    }
    //2
    HASH_ITER(hh, hashTable, item, tmpItem) {
        HASH_DEL(hashTable, item);
            free(item);
    }
    *returnSize = 0; // này thì ta đọc *returnSize = 2 khi tìm thấy 2 số, thì bây giờ không tìm thấy gì cả nên ta phải báo kích thước là 0 để hệ thống biết mà không cố đọc rác từ mảng 
    return malloc(sizeof(int) * 0); // trả về một mảng rổng cho hàm chính 0 bytes á 
}
```        
