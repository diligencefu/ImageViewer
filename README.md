# ImageViewer
朋友圈列表，动态列表，图片显示，图文显示，图片查看器
时
Image-Viewer
也是七月份的时候，因为换了公司，被迫开始使用swift，现在虽然不算是特别精通，但是配合Google写一个正常的项目还是没问题。 公司做了一个整形医院的APP，里面有一个类似于朋友圈动态的功能，找了一圈才找到一个合适的图片查看器，感觉还不错，所以就配合着我的动态圈一块上传来了。
GitHub地址
核心代码： 
override func tableView(_ tableView: UITableView, cellForRowAt indexPath: IndexPath) -> UITableViewCell {
    let cell : TableViewCell = tableView.dequeueReusableCell(withIdentifier: identyfierTable1, for: indexPath) as! TableViewCell    
    cell.selectionStyle = .default        
    var num = Int(arc4random_uniform(6))+1    
    if num == 0 {       
    num = 7   
    }        
    if indexPath.row == 0 {      
    num = 8    
    }           
    cell.assignToViews(num: num)    
    cell.showImageAction = {               
    self.theImage = $2       
    self.theIndex = $0               
    let bview = HJImageBrowser()              
    bview.delegate = self
// bottomView 这个一定要填写你点击的imageView的直接父视图 // 
cell.viewWithTag(10086) 这个就是cell类里面的那个images（UIView）我在Xib里面设置的 
bview.bottomView = cell.viewWithTag(10086)
// 当前点击的图片在该数组中的位置。 
bview.indexImage = $0
bview.defaultImage = $2               
bview.arrayImage = $1 as! [String]                
bview.show()           
}        
return cell
}

override func tableView(_ tableView: UITableView, didSelectRowAt indexPath: IndexPath) {    
tableView.deselectRow(at: indexPath, animated: true)    
}//   
#MARK:HJImageBrowserDelegatefunc getTheThumbnailImage(_ indexRow: Int) -> UIImage {
// 点击图片之后，放大过程中显示的那张图片 
return theImage }
}
func assignToViews(num:NSInteger) { 
theNum = num
    //        清除之前添加的imageView    _ = images.subviews.map {        $0.removeFromSuperview()    }
// let randomNumberTwo:Int = Int(arc4random_uniform(6))+1
    contentLabel.text = typeArr[num] as? String
// contentLabel.numberOfLines 我们的要求是最多三行 如果需要全部显示的话，可以设置为0
// contentLabel.numberOfLines = 0
// 运用九宫格排序来对图片进行排列 
let kSpace = CGFloat(7)//图片之间的间隙 
let kHeight = CGFloat(176kSCREEN_SCALE) //图片高度
let kWidth = (images.frame.size.width-kSpace2)/3
let lines = 3 //有多少列（每行要显示几张图片）    
for index in 0...num - 1{           
let row =  CGFloat(index%lines) //当前图片应该在第几行               
let imageV = UIImageView.init(frame: CGRect(xkWidth + kSpace) * row, ykSpace+kHeight)*CGFloat(index/lines), width: kWidth, height: kHeight))
// 利用Kingfisher加载图片 imageV.kf.setImage(with: URL(string:imageArr[index] as! String)!, placeholder: #imageLiteral(resourceName: "headIcon"), options: nil, progressBlock: nil, completionHandler: nil) 
imageV.tag = index 
imageV.contentMode = .scaleAspectFill //图片的填充方式 
imageV.clipsToBounds = true
imageV.isUserInteractionEnabled = true //用户交互一定要打开，否则手势不会响应
// 添加点击手势 
let singleTap = UITapGestureRecognizer.init(target: self, action: #selector(viewTheBigImage(ges)) singleTap.numberOfTapsRequired = 1 
imageV.addGestureRecognizer(singleTap)
images.addSubview(imageV)
// print(imageV.frame) 
}

// 最后，更新一下放图片的控件的高度 // CGFloat(num/3+1)kHeight+kSpaceCGFloat(num/3) var height = 0
    let line = CGFloat(num%3)        
    if line == 0 {       
    height = Int(CGFloat(num/3)*kHeight + kSpace*CGFloat(num/3))   
    }else{        
    height = Int(CGFloat(num/3+1)*kHeight + kSpace*CGFloat(num/3))    
    }         
    images.snp.updateConstraints { (ls) in       
    ls.height.equalTo(height)    
    }
    }
    @objc func viewTheBigImage(ges:UITapGestureRecognizer) { 
    let imageView = ges.view as! UIImageView           
    let showImages = NSMutableArray()       
    for index in 0...theNum-1 {    
    showImages.add(imageArr[index])   
    }      
    //        进入图片查看器的时候，必要参数，在这里直接传递过去       
    //        真正进到项目里面的时候，不必这么麻烦，直接把Model传回去，包括所有   
    if showImageAction != nil {        
    showImageAction!((ges.view?.tag)!,showImages as! Array<Any>,imageView.image!)   
  }
  }
  @IBAction func addAttention(_ sender: UIButton) {    
  setToast(str: "加关注")
  }
  
  @IBAction func commentAction(_ sender: UIButton) {    
  setToast(str: "评论一下")
  }
@IBAction func likeAction(_ sender: UIButton) {        
if sender.titleLabel?.text?.characters.count == 6 {      
setToast(str: "你赞了他一下")       
sender.setImage(#imageLiteral(resourceName: "red_zan"), for: .normal)     
sender.setTitle("已赞 · 100", for: .normal)      
sender.setTitleColor(kMainColor(), for: .normal)   
}else{      
setToast(str: "你取消了赞")       
sender.setImage(#imageLiteral(resourceName: "zan"), for: .normal)        
sender.setTitle("赞 · 99", for: .normal)       
sender.setTitleColor(kGaryColor(num: 100), for: .normal)    }  
}

