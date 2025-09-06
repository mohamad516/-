# -/**
 * Homepage for account selling website
 * Displays featured accounts, categories, and search functionality
 */

import { useState } from 'react'
import { Search, ShoppingCart, Gamepad2, Film, Music, Zap } from 'lucide-react'
import { Button } from '@/components/ui/button'
import { Input } from '@/components/ui/input'
import AccountCard from '@/components/AccountCard'
import CategoryFilter from '@/components/CategoryFilter'

// Account type definition
interface Account {
  id: number
  title: string
  price: number
  category: string
  description: string
  features: string[]
  image: string
  rating: number
  stock: number
}

// Sample account data
const sampleAccounts: Account[] = [
  {
    id: 1,
    title: 'اکانت Netflix Premium',
    price: 25,
    category: 'streaming',
    description: 'اکانت نت‌فلیکس پریمیوم با دسترسی به تمام محتواها',
    features: ['4K Ultra HD', '4 دستگاه همزمان', 'دانلود آفلاین'],
    image: 'https://pub-cdn.sider.ai/u/U05XHA57ELG/web-coder/68bcc41cd4f5bb9dcd3bfeb2/resource/17b5e147-f167-4267-874a-5fff9ad67f07.jpg',
    rating: 4.8,
    stock: 15
  },
  {
    id: 2,
    title: 'اکانت بلاکس فروت',
    price: 50,
    category: 'gaming',
    description: 'اکانت استیم با مجموعه بازی‌های محبوب',
    features: ['100+ بازی', 'آنلاین مولتی‌پلیر', 'به‌روزرسانی رایگان'],
    image: 'https://pub-cdn.sider.ai/u/U05XHA57ELG/web-coder/68bcc41cd4f5bb9dcd3bfeb2/resource/5530bd68-1728-48a5-b6eb-569dccb822ac.jpg',
    rating: 4.9,
    stock: 8
  },
  {
    id: 3,
    title: 'اکانت Spotify Premium',
    price: 15,
    category: 'music',
    description: 'اکانت اسپاتیفای پریمیوم بدون تبلیغات',
    features: ['بدون تبلیغات', 'دانلود آفلاین', 'کیفیت صوتی بالا'],
    image: 'https://pub-cdn.sider.ai/u/U05XHA57ELG/web-coder/68bcc41cd4f5bb9dcd3bfeb2/resource/1ff1be5a-7851-4c9f-84d1-dace5cf86f3b.jpg',
    rating: 4.7,
    stock: 20
  },
  {
    id: 4,
    title: 'اکانت Adobe Creative Cloud',
    price: 35,
    category: 'software',
    description: 'تمام نرم‌افزارهای ادوبی در یک پکیج',
    features: ['Photoshop', 'Premiere Pro', 'After Effects', '20+ ابزار دیگر'],
    image: 'https://pub-cdn.sider.ai/u/U05XHA57ELG/web-coder/68bcc41cd4f5bb9dcd3bfeb2/resource/836cff88-f73c-474f-bb28-66cbc0235b58.jpg',
    rating: 4.6,
    stock: 5
  }
]

const categories = [
  { id: 'all', name: 'همه', icon: Zap },
  { id: 'gaming', name: 'گیم', icon: Gamepad2 },
  { id: 'streaming', name: 'استریم', icon: Film },
  { id: 'music', name: 'موسیقی', icon: Music },
  { id: 'software', name: 'نرم‌افزار', icon: Zap }
]

export default function HomePage() {
  const [accounts] = useState<Account[]>(sampleAccounts)
  const [filteredAccounts, setFilteredAccounts] = useState<Account[]>(sampleAccounts)
  const [selectedCategory, setSelectedCategory] = useState('all')
  const [searchQuery, setSearchQuery] = useState('')
  const [cartItems, setCartItems] = useState<Account[]>([])

  // Filter accounts based on category and search query
  const filterAccounts = (category: string, query: string) => {
    let filtered = accounts

    if (category !== 'all') {
      filtered = filtered.filter(account => account.category === category)
    }

    if (query.trim() !== '') {
      filtered = filtered.filter(account =>
        account.title.toLowerCase().includes(query.toLowerCase()) ||
        account.description.toLowerCase().includes(query.toLowerCase())
      )
    }

    setFilteredAccounts(filtered)
  }

  const handleCategoryChange = (category: string) => {
    setSelectedCategory(category)
    filterAccounts(category, searchQuery)
  }

  const handleSearchChange = (query: string) => {
    setSearchQuery(query)
    filterAccounts(selectedCategory, query)
  }

  const addToCart = (account: Account) => {
    setCartItems(prev => [...prev, account])
  }

  return (
    <div className="min-h-screen bg-gradient-to-br from-gray-50 to-gray-100">
      {/* Header */}
      <header className="bg-white shadow-sm sticky top-0 z-50">
        <div className="container mx-auto px-4 py-4">
          <div className="flex items-center justify-between">
            <div className="flex items-center space-x-2">
              <Zap className="h-8 w-8 text-blue-600" />
              <h1 className="text-2xl font-bold text-gray-900">اکانت بلاکس فروت</h1>
            </div>
            
            <div className="flex items-center space-x-4">
              <div className="relative">
                <Search className="absolute left-3 top-1/2 transform -translate-y-1/2 text-gray-400 h-4 w-4" />
                <Input
                  placeholder="جستجوی اکانت..."
                  className="pl-10 w-64"
                  value={searchQuery}
                  onChange={(e) => handleSearchChange(e.target.value)}
                />
              </div>
              
              <Button variant="outline" className="bg-transparent relative">
                <ShoppingCart className="h-5 w-5" />
                {cartItems.length > 0 && (
                  <span className="absolute -top-2 -right-2 bg-red-500 text-white text-xs rounded-full h-5 w-5 flex items-center justify-center">
                    {cartItems.length}
                  </span>
                )}
              </Button>
            </div>
          </div>
        </div>
      </header>

      {/* Hero Section */}
      <section className="bg-gradient-to-r from-blue-600 to-purple-600 text-white py-16">
        <div className="container mx-auto px-4 text-center">
          <h2 className="text-4xl font-bold mb-4">بهترین اکانت‌ها با بهترین قیمت</h2>
          <p className="text-xl mb-8 opacity-90">اکانت‌های اورجینال با پشتیبانی 24 ساعته</p>
          <Button size="lg" className="bg-white text-blue-600 hover:bg-gray-100">
            مشاهده همه اکانت‌ها
          </Button>
        </div>
      </section>

      {/* Main Content */}
      <main className="container mx-auto px-4 py-8">
        {/* Categories */}
        <CategoryFilter
          categories={categories}
          selectedCategory={selectedCategory}
          onCategoryChange={handleCategoryChange}
        />

        {/* Accounts Grid */}
        <div className="grid grid-cols-1 md:grid-cols-2 lg:grid-cols-3 xl:grid-cols-4 gap-6 mt-8">
          {filteredAccounts.map(account => (
            <AccountCard
              key={account.id}
              account={account}
              onAddToCart={addToCart}
            />
          ))}
        </div>

        {filteredAccounts.length === 0 && (
          <div className="text-center py-12">
            <p className="text-gray-500 text-lg">هیچ اکانتی با این مشخصات پیدا نشد</p>
          </div>
        )}
      </main>

      {/* Footer */}
      <footer className="bg-gray-900 text-white py-8 mt-16">
        <div className="container mx-auto px-4">
          <div className="grid grid-cols-1 md:grid-cols-3 gap-8">
            <div>
              <h3 className="text-lg font-semibold mb-4">درباره ما</h3>
              <p className="text-gray-400">
                ما ارائه‌دهنده اکانت‌های اورجینال با بهترین قیمت و پشتیبانی 24 ساعته هستیم
              </p>
            </div>
            <div>
              <h3 className="text-lg font-semibold mb-4">لینک‌های مفید</h3>
              <ul className="space-y-2 text-gray-400">
                <li><a href="#" className="hover:text-white">راهنمای خرید</a></li>
                <li><a href="#" className="hover:text-white">پشتیبانی</a></li>
                <li><a href="#" className="hover:text-white">ضمانت بازگشت</a></li>
              </ul>
            </div>
            <div>
              <h3 className="text-lg font-semibold mb-4">تماس با ما</h3>
              <p className="text-gray-400">
                پشتیبانی 24 ساعته<br />
                ایمیل: support@example.com<br />
                تلگرام: @example
              </p>
            </div>
          </div>
          <div className="border-t border-gray-800 mt-8 pt-8 text-center text-gray-400">
            <p>&copy; 2024 اکانت بلاکس فروت. تمامی حقوق محفوظ است.</p>
          </div>
        </div>
      </footer>
    </div>
  )
}

اینجا میتونی بهترین اکانت های بلاکس فروت رو خریداری کنی
